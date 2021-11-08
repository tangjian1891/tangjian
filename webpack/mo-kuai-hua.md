# 模块化

### 曾经:是没有模块化的

所有js脚本全跑在全局上下文中:会导致污染全局变量，依赖顺序混乱。

### 以前:临时解决方式:[模块写法](http://www.ruanyifeng.com/blog/2012/10/javascript\_module.html)

1. 对象写法:会暴露所有成员。
2. IIFE立执行函数:做闭包，可以不暴露函数内的私有成员变量，没有模块化时比较好的方式。

### 现代:标准模块化（ESM）

> 特点:静态加载(编译时分析),有利于类型检查,tree shaking。未来标准

#### 1.export 输出的{}不是对象，不是对象，不是对象。而是一种接口规范,对象写法不符合规范

```
let msg2=123
export {
  msg:"数据", //不合法
  msg2:msg2, //不合法
  msg3:msg2, //不合法
}
```

#### 2.import 命令引入的接口变量都是read-only只读的。理解为const定义(所以如果值是引用类型对象，仍然可以对内部属性改值)。但是建议引入的变量，不要轻易改变它的属性

```
import { num,obj } from './a.js'
obj.age=26 // success正常运行,但是不推荐这么搞，不好查错
num=10 //报错: Uncaught TypeError: Assignment to constant variable.
```

#### 3.ESM 输出的是接口，与其对应的值是动态绑定关系。通过接口，可以取到内部模块实时的值,所以模块内部的数据变动后，可以在引入的地方实时拿到变动的值(基本类型的变动也是可以的)。

#### 4.export default 实际上默认导出的一个default变量。export 与 export default是可以共存的。可以使用整体加载的方式查看。所以本质上ESM都是依赖于export导出

```
import * as test from './test.js' 如果test模块有export default导出，那么test对象上就有default属性。


import test from './test.js' 此写法会默认查找default属性然后赋值到test上
import {default as test} from './test.js' 此写法与上面效果一致，手动拿default

import test,{num} from './test.js' 此写法可拿default属性与单个export
```

#### 扩展:import()

想在运行时加载模块，使用静态的import 导入语法就不行了。import是无法取代require的动态加载功能。

[ES2020提案](https://github.com/tc39/proposal-dynamic-import)，引入import()函数，支持动态加载模块.import()为异步加载，Node的require为同步。

实践：业务懒加载模块。



### 混用解决办法:

#### CommonJS模块加载ES6模块。

不能使用require(),require()是同步的，不能直接加载ES6模块。ES6模块内部可以使用顶层await，无法被同步加载。所以只能折中这种采用import()函数，import()是2020新特性标准。

```
(async () => {
  await import('./a.mjs');
})();
```

ES6模块加载CommonJS模块

import可以直接加载CommonJS模块，但是只能整体加载，不能单独加载某一项。因为CommonJS模块导出的对象对ESM来说默认就在default上，相当于export default导出 .

```
 import * as a from './test.js'; //ES6加载的路径必须是完整的路径
console.log(a);  //{ default: { msg: 'CommonJS导出的就是对象' } }


import a from './test.js';
console.log(a);   //{ msg: 'CommonJS导出的就是对象' }
```

### 模块循环加载问题

CommonJS循环引用问题

加载CommonJS模块文件时，会在内存中标记这个模块文件

```
{
  id: '...',
  exports: { ... },//已经暴露的数据会在这里
  loaded: true, //关键标识，文件多次引用时，只会被加载一次。且只要开始执行就变为true
  ...
}
```

[官网Cycles示例](https://nodejs.org/api/modules.html#modules\_cycles)

```
// a.js 从此开始执行
exports.done = false;//进入时，loaded已经被标记为true了
var b = require('./b.js'); //开始解析b.js文件

console.log('在 a.js 之中，b.done = %j', b.done);//当b.js完全解析完成后，继续解析剩下的
exports.done = true;
console.log('a.js 执行完毕');
```

```
// b.js
exports.done = false;
var a = require('./a.js'); //前往加载a.js文件，发现a.js已经被标记为true，直接查找数据，发现done=false
console.log('在 b.js 之中，a.done = %j', a.done);
exports.done = true;
console.log('b.js 执行完毕');
```

结果:

```
在 b.js 之中，a.done = false
b.js 执行完毕
在 a.js 之中，b.done = true
a.js 执行完毕
```

ESM循环加载问题

```
// a.mjs
import { bar } from "./b.mjs";
console.log("a.mjs");
console.log(bar);
export var foo = "foo";

```

```
// b.mjs
import {foo} from './a.mjs';
console.log('b.mjs');
console.log(foo);
export let bar = 'bar';
```

结果 .可以尝试修改export var修改为let,以及改为函数导出形式。形式很多

```
b.mjs
undefined
a.mjs
bar
```

ESM会优先执行所有import语句。在b.js执行import a.js时，会发现a虽然没有被解析完成，但是会被标记为Fectching（有点像CommonJS的loaded做标记）。因为静态分析的原因，所以a.js导出的变量如果不存在变量hoisting提升(var申明，函数申明)，那么将报错(例如:let const 声明)。其中:var申明会正常执行，但是取不到值为undefined,而函数将可以正常执行。

[Hoisting变量提升](https://developer.mozilla.org/zh-CN/docs/Glossary/Hoisting) JavaScript在执行任何代码之前，会将函数声明放入内存中。函数和变量申明，会先放函数。

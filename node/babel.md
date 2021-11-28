---
description: babel是一个javascript的compiler编译器
---

# Babel

工作流程:解析输入的源码转为AST抽象语法树，对AST做转换，最后对转换后的AST做代码生成，输出转换后的代码。

![](<../.gitbook/assets/image (2).png>)

babel的使用场景。就我现在有多种地方使用，浏览器，rollup,webpack,vue-cli,vite

TC39将[提案流程](https://tc39.es/process-document/)分为以下[几个阶段](https://github.com/tc39/ecma262):

[Stage-0](https://github.com/tc39/proposals/blob/master/stage-0-proposals.md):任何人都可以提出自己的想法，没有门槛

[Stage-1](https://github.com/tc39/proposals/blob/master/stage-1-proposals.md):想法有一定可行性，值得做,出现一个正式的提案。

[Stage-2](https://github.com/tc39/proposals#stage-2):初始的草案规范。（装饰器还卡这）

[Stage-3](https://github.com/tc39/proposals#stage-3):完整的草案规范出炉，浏览器厂商开始实现。（基本上到了Stage-3就可以开始放心使用了）

[Stage-4](https://github.com/tc39/proposals/blob/HEAD/finished-proposals.md#finished-proposals):下一个版本正式发行


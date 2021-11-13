# typescript

### 安装

安装之后。统一使用tsc简写替代typescript。例如查询版本，编译文件

```
npm i typescript -g
tsc -v
```

1.全目录编译，需要配置文件

```
tsc --init
tsc   //在有配置文件的情况下，tsc可以直接全目录下编译
```

2.指定编译的文件，不需要配置文件，需要指向目标文件目录

```
tsc ./main.ts    //没有配置文件，就只能一个一个指定路径编译
```

3.使用vsocde插件run code直接运行，需要借助一个ts-node全局包，否则会有乱码

4.实时编译。每次手动都太麻烦了，如果能watch文件变化。增加-w参数即可



### rollup+typescript

rollup插件大[地址](https://github.com/rollup/plugins)，内部有typscript

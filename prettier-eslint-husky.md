# prettier/eslint/husky

## prettier

格式化工具(不检查语法错误)代码缩进工具.

分为两种，两种都会自动查询配置

vcode手动格式化 :安装prettier插件，使用alt+shift+f手动格式化

[npm包指令格式化](https://www.prettier.cn/docs/install.html):配置npx prettier --write .  指令格式化

配置抓取优先顺序:.prettierrc文件 > .vscode/settings >全局settings

### .prettierrc  文件 除了一个printWidth,其余都是默认

```
{
  "printWidth": 150,
  "tabWidth": 2,
  "semi": true,
  "singleQuote": false,
  "trailingComma": "all",
  "bracketSpacing": true,
  "arrowParens": "always"
}

```

### .prettierignore 忽略文件

```
# 忽略格式化文件 (根据项目需要自行添加)
node_modules
dist
.vscode
public
# 如果需要忽略某些内容，可以添加注释 // prettier-ignore
```

### 工程化项目使用

```
//.prettierrc与.prettierignore配置文件增加

npm i prettier -D            //依赖增加

"prettier": "prettier --write ."            //package/scripts脚本增加
```


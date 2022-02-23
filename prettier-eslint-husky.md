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

### script脚本

npm i prettier -D

```
  "scripts": {
    "prettier": "prettier --write ."
  },
```

## eslint

eslint有校验检查，修复的能力，但是一般来说我不希望修复那些与prettier相似的，避免冲突

### .eslintrc.js

```
{
  "env": {
    //当前最高为2021   https://eslint.org/docs/user-guide/configuring/language-options#specifying-environments
    "browser": true,
    "node": true,
    "es2021": true
  },
  // "global":{}, //定义全局变量，一般来说用不到
  "parserOptions": {
    "sourceType": "module" //允许esm模块的使用
  },
  "extends": ["eslint:recommended", "prettier"],
  "rules": {}
}

```

### .eslintignore

```
*.sh
node_modules
*.md
*.woff
*.ttf
.vscode
.idea
dist
/public
/docs
.husky
.local
/bin
Dockerfile

```

### script脚本

npm i eslint -D

```
  "scripts": {
    "lint": "eslint src/**/*.{js,vue,ts} --fix",
  },
```

## 融合eslint+prettier

npm i eslint-config-prettier -D

然后在.eslintrc.js文件中增加到extends扩展的最后面

## husky+lint-staged

husky主要是扩展原生git得hook。使得每次提交时，可以动态执行命令

lint-staged主要是仅对暂存区的文件进行校验。因为每次都对全量文件lint，可能破坏性太大了

安装[husky](https://typicode.github.io/husky/#/?id=install)

```
npm i husky -D 
npx husky install            会生成.husky/ 文件目录，但是还没有pre-commit
npm set-script prepare "husky install"        装入prepare，成员每次npm i时，自动载入
npx husky add .husky/pre-commit "npx lint-staged"    生成pre-commit钩子并写入调用脚本
```

注意一个点：**yarn 安装是不支持 prepare 这个生命周期的，需要将 prepare 改成 postinstall**

安装lint-staged

```
npm i lint-staged -D
#在package.json中配置好lint-staged将要执行得内容，lint-staged会被pre-commit中调用

  "lint-staged": {
    "{src/**/*.{js,vue,ts},index.html}": [
      "eslint --fix",
      "prettier --write"
    ]
  }
```

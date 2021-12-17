---
description: 个人常用的一些vscode配置
---

# vscode软件

下载**System Installer全系统用户版本**

C:\Users\Administrator\AppData\Roaming\Code\User**   配置所在路径，有snippets,sync之类**

全局settings.json

记得切换一下终端

```
{
  "editor.tabSize": 2, //每个tab 几个空格
  "editor.fontSize": 18, //字体大小
  // vscode终端设置
  "terminal.integrated.profiles.windows": {
    "PowerShell": {
      "source": "PowerShell",
      "icon": "terminal-powershell"
    },
    "Command Prompt": {
      "path": ["${env:windir}\\Sysnative\\cmd.exe", "${env:windir}\\System32\\cmd.exe"],
      "args": [],
      "icon": "terminal-cmd"
    },
    "Git Bash": {
      "source": "Git Bash"
    },
    "Windows PowerShell": {
      "path": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe"
    }
  },
  "terminal.integrated.defaultProfile.windows": "Windows PowerShell",
  // ------------上面为vscode本身配置---------

  
  // ----------下面为插件的配置-------------
  "workbench.iconTheme": "vscode-icons", //vscode 文件icon使用插件
  "workbench.colorTheme": "One Dark Pro Darker", //vscode 主题插件

  "prettier.printWidth": 150,

  "vetur.ignoreProjectWarning": true, //忽略vetur检测vetur.congig.js/ts配置信息提示
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}

```

1. Chinese &#x20;
2. Code Runner
3. Auto Rename Tag
4. Bracket Pair Colorizer 2
5. Live Server
6. Open in Browser
7. GitLens — Git supercharged 可查看上次上提交
8. Git History&#x20;
9. Comment Translate  翻译注释，原来看源码英文注释，可以切换翻译源
10. Vetur 可全局开启
11. Volar 建议全局关闭，再vue3的项目再开启，开启时记得关闭Vetur关闭V
12. vscode-icons  (需要配置)
13. One Dark Pro  (需要配置)
14. Prettier  (使用默认配置唯一需要调整的就是单行长度)（放弃vetur的格式，.vue使用prettier格式化，配合eslint fix即可，主要问题就是折行属性关掉vue/max-attributes-per-line即可）
15.

非必须插件，使用时再装也行

1. compareit  对比文件差异插件
2. Paste JSON as Code  可以将JSON转TS等等
3. VS Code Counter   统计项目代码量
4. Import Cost  查看引入包大小

### prettier配置

自建 .prettierrc 文件

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

### vue2项目的eslint配置

可供参考的[vetur默认格式化](https://vuejs.github.io/vetur/guide/formatting.html#formatters)，不过不需要了,.vue使用prettier格式化，与vue3统一

```
module.exports = {
  root: true,
  env: {
    browser: true,
    node: true,
    es6: true,
  },
  extends: [
    "plugin:vue/essential", //基本配置，自带 针对.vue文件
    "plugin:vue/strongly-recommended", //强烈推荐  手动增加 针对 .vue文件
    "eslint:recommended", //eslint 基本配置  针对 js
  ],
  parserOptions: {
    parser: "babel-eslint",
  },
  rules: {
    "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",
    "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off",

    // 1.1 js中默认双引号 warn 级别 【自动fix】
    quotes: [1, "double"],

    // 1.2 强制分号 warn 级别 【自动fix】
    semi: [1, "always"],

    // 1.3 多行属性时强制增加 逗号  warn 级别 【自动fix】
    "comma-dangle": [
      1,
      {
        arrays: "always-multiline",
        objects: "always-multiline",
        imports: "always-multiline",
        exports: "always-multiline",
        functions: "never",
      },
    ],

    // 1.4 关闭强制使用全等 http://eslint.cn/docs/rules/eqeqeq
    eqeqeq: [0],

    // 2.2 .vue 的name属性 warn 使用短横线 【自动fix】
    "vue/name-property-casing": ["warn", "kebab-case"],

    // 2.3 Vue组件中Prop名称强制使用驼峰 error 级别
    "vue/prop-name-casing": [2, "camelCase"],

    // 2.4 prop定义应始终尽可能详细，至少指定类型 不允许用数组接收 .error 级别
    "vue/require-prop-types": [2],

    // 2.5 使用组件时，强制 <CoolComponent> 大写  warn 级别 【自动fix】
    "vue/component-name-in-template-casing": [1, "PascalCase"],

    // 2.6 组件中options定义的顺序  遵循vue风格指南 warn 级别 【自动fix】
    "vue/order-in-components": [
      1,
      {
        order: [
          "el",
          "name",
          "parent",
          "functional",
          ["delimiters", "comments"],
          ["components", "directives", "filters"],
          "extends",
          "mixins",
          "inheritAttrs",
          "model",
          ["props", "propsData"],
          "fetch",
          "asyncData",
          "data",
          "computed",
          "watch",
          "LIFECYCLE_HOOKS",
          "methods",
          "head",
          ["template", "render"],
          "renderError",
        ],
      },
    ],

    // 2.7 属性顺序,遵循vue风格指南 warn 级别 【自动fix】
    "vue/attributes-order": [
      1,
      {
        order: [
          "DEFINITION",
          "LIST_RENDERING",
          "CONDITIONALS",
          "RENDER_MODIFIERS",
          "GLOBAL",
          "UNIQUE",
          "TWO_WAY_BINDING",
          "OTHER_DIRECTIVES",
          "OTHER_ATTR",
          "EVENTS",
          "CONTENT",
        ],
        alphabetical: false, //关闭按字母顺序
      },
    ],
    //  关闭 prop属性强制需要默认值
    "vue/require-default-prop": [0],

    //  关闭 强制实行单标签
    "vue/html-self-closing": [0],

    // 关闭标签属性强制使用短横线。
    "vue/attribute-hyphenation": [0],

    // 关闭单行html折行
    "vue/singleline-html-element-content-newline": 0,

    // 未使用 变量进行警告，不error
    "no-unused-vars": [
      1,
      {
        vars: "all", //检测所有变量
        args: "none", //参数未使用，可不检测
        caughtErrors: "none", //不检测catch中的
        // argsIgnorePattern:'^_',//忽略_开头的
      },
    ],
    "no-useless-catch":[0],
    "no-inner-declarations":[0], // 函数申明可以放在函数的最下方
    "require-atomic-updates":[0]

    // 不推荐开的
    //2.1 template 属性折行 warn 单行超过5个就自动折行 【自动fix】,使用prettier就行了
    // "vue/max-attributes-per-line": [
    //   1,
    //   {
    //     singleline: 5,
    //     multiline: {
    //       max: 1,
    //       allowFirstLine: false,
    //     },
    //   },
    // ],
  },
};

```

### 工作区建议（安装插件）

```
{
  "recommendations": [
    "formulahendry.auto-rename-tag",
    "coenraads.bracket-pair-colorizer-2",
    "ms-ceintl.vscode-language-pack-zh-hans",
    "formulahendry.code-runner",
    "intellsmi.comment-translate",
    "eamodio.gitlens",
    "ritwickdey.liveserver",
    "zhuangtongfa.material-theme",
    "techer.open-in-browser",
    "esbenp.prettier-vscode",
    "octref.vetur",
    "vscode-icons-team.vscode-icons",
    "johnsoncodehk.volar"
  ]
}
```

### 常用软件（电脑）

#### git

创建ssh-key

```
 ssh-keygen -t rsa -C "tangjian1891@163.com"
```

#### sourcetree

SourceTree 登录 Bitbucket。外网，所有要等一会。账号密码为邮箱 1.装好后，直接配置ssh 私钥（工具-》选项-》SSH客户端配置 (选择OpenSSH)） 2.连接远程仓库github 选择https协议 OAuth验证方式

#### powershell

[https://blog.csdn.net/qq\_30376375/article/details/116139870](https://blog.csdn.net/qq\_30376375/article/details/116139870) windows powerShell 配置

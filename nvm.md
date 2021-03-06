---
description: >-
  npm版本管理工具，快速切换版本，便于学习。 nvm不支持windows系统，所以需要使用nvm-windows
  下载nvm-setup.zip，此版本无须配置环境变量
---

# nvm

### nvm下载

> 下载地址[nvm-windows](https://github.com/coreybutler/nvm-windows/releases)  选择 [nvm-setup.zip](https://github.com/coreybutler/nvm-windows/releases/download/1.1.7/nvm-setup.zip) .
>
> 默认目录:     C:\Users\Administrator\AppData\Roaming\nvm   这是我的地址，默认安装目录请自行留意安装时的提示

### 设置nvm镜像全局

```
nvm node_mirror https://npm.taobao.org/mirrors/node/
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
```

### 常用指令

常用指令:

1. nvm list 查看已安装的node版本
2. nvm use \[version] 在已有list版本中切换,如遇切换版本无效时，重新安装一次nvm包
3. nvm list available 查看所有可用版本
4. nvm install latest 安装最新版本
5. nvm install \[version] 安装指定版本
6. nvm uninstall \[version] 卸载指定版本

### npm(适合安装全局包)

> node包管理自带npm版本，所以下载node后可直接使用npm。最新的node16->npm7,其余常规npm版本应该为6。

设置npm镜像(强烈推荐)\
常用指令:

```
npm get registry                                         查看当前的镜像源版本
npm set registry https://registry.npmmirror.com          设置淘宝镜像(好像直接兼容yarn)

npm set registry https://registry.npmjs.org              设置为官方镜像
```

发布操作

```
npm list           查看当前目录下安装了哪些node包
npm list -g        查看全局node包
npm who            查看当前登录账号
npm login          登录账号（切换到官方镜像源才可以）
npm publish        发布包
```

查看路径

```
npm get prefix         查看node安装路径
npm root -g            全局node包路径
```

清理缓存

```
npm cache clean -f
```

### yarn(适合安装项目依赖)

```
yarn config get registry 
yarn config set registry https://registry.npmmirror.com       设置淘宝镜像（好像没用，不如设置npm的）
   
yarn config set registry https://registry.yarnpkg.com         切换为官方(好像没啥用)
```

### package.lock.json&#x20;

起因: 1.项目临近上线和老大优化webpack时发现使用不同的安装依赖，run build出来的大小在gzip下有接近80kb的差距。cnpm npm yarn三种方式（开始怀疑node\_modules）\
2.你是否有从github上clone项目到本地，但是跑步不起来的情况?很有可能就是这个原因。\
先结论:\
1.推荐使用npm改镜像源为淘宝使用。\
2.不要使用cnpm,这玩意会无视package.lock.json文件，会根据\["^","\~"]下载当前大版本，小版本下的最新依赖包\
3.**\["\~"]小版本**升到最新 \~1.1.1,会自动寻找1.1.x的最新。但是并不会到1.2.x\
4.**\["^"]中版本**升到最新 ^1.1.1,会自动寻找1.x的最新。但并不会到2.x\
5.推荐一直推送package.lock.json到项目工程中，确保团队成员版本依赖一致性。(当然注意定期检查)\
6\. 如果package.json与package.lock.json中依赖的版本对不上，那么会自动根据\["^","\~"]更新到最新的latest,并把最新的版本号更新到lock中（package.json中不会被更改）。所以这样会引发一个问题，只要你一个版本对不上，那么后续三方依赖发布新版本，只要你npm i ，那么就会重新下载最新的依赖。\
7\. 强制对齐版本的方法。\
7.1.npm i vant@\[version] 安装指定版本，可以刷新lock.json\
7.2.前往package.json中去掉\["^","\~"],强锁版本后，再npm i\
7.3.手动更改lock.json中的版本号。（方法费劲），lock.json中仅仅直接替换版本号往往是不行的，还会检测hash依赖integrity。可以采取删除integrity,修改version,resolved中的版本号与package.json一致后，再npm i 一下。

建议项目工程强锁版本。（上线的东西）毕竟开源第三方，随时拉闸，就算第三方靠着住，第三方包的依赖包也说不准。

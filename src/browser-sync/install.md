## 安装
在使用工具之前首先要做的是安装，Browser-Sync 的安装很简单，它的依赖只有一个 Node.js 环境。Node.js 的安装这里就不介绍了，在安装好 Node.js 后，只需要使用 NPM 将 Browser-Sync 安装到全局环境即可。
```shell
$ npm install -g browser-sync
```
**注意：不要使用 `sudo` 来安装 Browser-Sync！** 如果在使用 NPM 全局安装某个包的时候出现了 EACCES 错误的话，说明当前的命令没有向 NPM 需要写入文件的目录进行操作的权限，这是可以解决的，参考 NPM 的[官方文档](https://docs.npmjs.com/getting-started/fixing-npm-permissions)就可以很快修复这个问题。

如果不需要将 Browser-Sync 安装到全局环境，只需要在某个具体项目中使用的话，也可以将它作为一个开发时的依赖项加入到项目的 package.json 文件中：
```shell
$ npm install browser-sync --save-dev
```

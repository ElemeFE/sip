## 使用命令行运行 Browsersync
使用 Browsersync 最简单的方法就是通过命令行了，Browsersync 为我们提供了一些命令供我们使用。如果我们不清楚某个指令的用法时，可以通过 `$ browser-sync <command> --help` 来获得命令的说明。
### `$ browser-sync start [options]`
这是启动 Browsersync 服务所要用的命令，它支持以下的参数：

参数                   | 含义
---------------------- | -------------
--server, -s           | 运行一个本地服务器（使用当前目录作为服务器的根目录）
--serveStatic, --ss    | 静态资源目录
--port	               | 指定要使用的端口
--proxy, -p	           | 代理到一个已有的服务器
--ws	                 | 只有 Proxy 模式下有效 - 支持 websocket 代理
--browser, -b          | 选择一个自动打开的浏览器
--files, -f            | 需要监听的目录
--index                | 指定用作首页的文件
--plugins              | 加载 Browsersync 插件
--extensions           | 指定文件扩展名的降级
--startPath            | 指定初始打开的地址路径
--https                | 打开本地开发环境的 SSL
--directory            | 展示服务器上的目录列表
--xip                  | 使用 xip.io 域名路由
--tunnel               | 使用一个公开 URL
--open                 | 选择自动打开的 URL，或提供一个 URL
--cors                 | 对所有请求加上 Access Control 头部
--config, -c           | 指定配置文件路径
--host                 | 指定使用的主机名
--logLevel             | 设置日志输出级别（silent, info 或 debug）
--reload-delay         | 文件修改到刷新事件的延迟时间，毫秒为单位
--reload-debounce      | 限制发送浏览器刷新事件到客户端的频率
--ui-port              | 指定 UI 界面使用的端口
--no-notify            | 关闭页面中的提示元素
--no-open              | 不打开新的浏览器窗口
--no-online            | 强制不联网
--no-ui                | 不打开用户界面
--no-ghost-mode        | 禁用 Ghost 模式
--no-inject-changes    | 所有文件修改都刷新页面
--no-reload-on-restart | 不在重启后自动刷新所有浏览器

下面我们来通过一个实际的例子来感受一下 Browsersync 的功能。这是一个非常简单的项目目录，一个 index.html 文件中引入了两个外联资源，分别是样式和脚本。

[]

我们希望以这个目录为服务器的根目录，当用户修改目录中任何一个文件时，都触发页面的更新。通过查阅上面的参数表格，我们可以知道需要用到的参数有 `--server` 和 `--file`：
```shell
# 以当前目录为服务器的根目录，并监听目录中所有文件的修改
$ browser-sync start --server --file *
```
通过运行这条命令，Browsersync 会自动在 localhost 的某个可用端口上开启服务，并自动在默认浏览器中打开页面。此时，我们可以打开多个不同的浏览器，访问同一个地址，并进行各种操作，如修改代码、滚动页面等。

[]

可以看到，我们在代码上的任何修改，都会实时触发页面的更新，不需要手动刷新，同时我们在页面上的任何操作都会同时反应到所有浏览器中。

当然，在实际项目的开发中，我们的构建工具一般已经为我们创建了 dev-server 并运行在本地的某个端口，或者通过 nginx 对某个域名进行了反向代理。因此我们不需要 Browsersync 再为我们开启这样的服务，直接访问现有的服务就可以了，我们可以将上面命令的 `--server` 参数换成 `--proxy` 并传入已有的服务地址，如 `localhost:xxxx`，相当于 Browsersync 在中间充当了一个代理层，同样可以达到要求：
```shell
$ browser-sync start --proxy 'localhost:1234' --files 'app'
```

另外，在实际项目中，项目的结构多种多样，需要传递给 Browsersync 的配置可能会很复杂。这个时候，将大量的配置参数写在一条命令里就显得有些笨拙了，我们可以将和一条命令相关的配置参数统一写在一个配置文件里，再通过命令行的 `--config` 参数传入配置文件路径，让 Browsersync 自己去读取配置就可以了。

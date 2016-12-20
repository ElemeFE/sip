## 使用命令行运行 Browser-Sync
使用 Browser-Sync 最简单的方法就是通过命令行了，Browser-Sync 为我们提供了一些命令供我们使用。如果我们不清楚某个指令的用法时，可以通过 `$ browser-sync <command> --help` 来获得命令的说明。
### `$ browser-sync start <options>`
这是启动 Browser-Sync 服务所要用的命令，它支持以下的参数：

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
--plugins              | 加载 Browser-Sync 插件
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

下面我们来通过一个实际的例子来感受一下 Browser-Sync 的功能。

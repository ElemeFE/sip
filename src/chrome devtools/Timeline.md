## Timeline
在日常开发中，timeline 使用频率可能不算高，事实上 timeline 提供了许多信息可供我们分析页面性能。当页面比较复杂，出现加载问题时，使用 timeline 来排查性能问题无疑是绝佳选择。  
图例Chrome 版本：55.0.2883.95

### Timeline Panel
分为4个部分：
1. **Controls** 可以进行操作，开始记录、结束记录，并且可以选择捕获什么信息。

2. **Overview** 页面性能的概览。

3. **Flame Chart** 火焰图，CPU 堆栈调用追踪的可视化。

4. **Details** 显示选中事件的具体信息，默认显示整体加载的信息。

![](http://p1.bqimg.com/567571/c74d559907b477dc.png)

#### Overview
概览分为3个部分，DevTools 里就标出来了。

1. **FPS** Frames Per Second，每秒刷新帧数。一般来说，我们要保证页面有高于每秒60帧的刷新速率。就是说浏览器对每一帧的渲染需要在 1 / 60 毫秒内完成。
图上绿色柱越高，fps越高。红色块是用时过长的帧，有可能导致页面[卡顿](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/timeline-tool)。

2. **CPU** CPU 资源。山形图显示了什么类型的事件消耗了 CPU 资源。

3. **NET** 每种颜色的条形代表了一种资源。长短代表了获取资源所需的时间。条形的浅色部分代表 requests waiting time。深色部分是 represents transfer time。

- 蓝色：HTML
- 黄色：Scripts
- 紫色：Stylesheets
- 绿色：Media
- 灰色：其他

![](http://p1.bpimg.com/567571/081abc6186f721f3.png)

### 如何开始记录
如果需要记录页面加载的情况需要先打开 DevTools 的 Timeline 面板，或者在 Timeline 面板按 Cmd + R(Mac)。
按下 Controls 的黑色 button 就自动开始记录啦。按钮为红色期间会记录页面交互。

#### tips
- 记录时间不要太长。太多的信息塞在小小的面板里也不方便。
- 避免不必要的操作。如果想要记录点击之后的事件，同时要避免在此期间滚动页面等等。
- 禁用浏览器缓存。
- 关闭扩展和插件。可以使用隐身模式或者在 chrome 中添加新的用户。

### capture 的选项
**Screenshots** 显示快照，略过不谈。

**Profile JavaScript** 剖析 js。 在 Controls 里打开 JS Profile 开关。在火焰图里可以看到每一个 js 函数的调用。 但是这个选项本身也会有一些消耗，因此，请在确保需要查看更多细节的时候才打开它。

![](http://p1.bpimg.com/567571/e335b411fbfc3781.png)  

蓝色、绿色、红色竖虚线分别代表 DOMContentLoaded，First paint，Load event 时间点。 

![](http://p1.bpimg.com/567571/e68ef99ec21ba352.png)

**Paint**
Enable Paint之后，选中 paint event 可以在 Detail 里看到 Paint Profiler 
在 More Tools => Rendering 里有更多 paint 的设置。

**Memory** 
- Documents 文档数
- Nodes Dom 节点数，保存在内存中尚待回收的节点。
- Listeners 事件监听计数

![](http://p1.bpimg.com/567571/0b8a7064484bd3d1.png)

**CPU throttling**  

和模拟不同网络情况一样，可以对 CPU 进行限制，模拟不同设备。

- - -
以上是具体的操作，得出数据以后要进行分析
- - -

### 分析
- 如图，红色是用时过长的帧，点击可以看到这段时间做了什么，这里只有3 fps = =。点击 function call 可以看到用时、哪句代码调用了事件，点击可以跳到 source 查看具体代码。
性能优化很可能就要从这里入手。定位到了具体位置再改善代码。

![](http://i1.piimg.com/567571/7d235defe214206a.png)

- 点击红色三角有浏览器的提示信息，给出了可能导致性能瓶颈的原因。

![](http://p1.bqimg.com/567571/11eede94f321c49a.png)

- 点击 Event Log，显示所有事件，可以根据自己的需要做筛选。

![](http://i1.piimg.com/567571/2f5ec32974de878c.png)

- Timeline 将事件分为不同类型，下面是事件的解释。

##### Loading Events 
事件 | 描述 |
----|------|
Parse HTML | 浏览器解析 HTML  |
Finish Loading | 网络请求结束  |
Receive Data | 请求的响应数据到达，如果数据较大，可能会触发多次  |
Receive Response | 响应头报文到达 |
Send Request | 开始发送请求 |

##### Scripts Events
事件 | 描述 |
----|------|
Animation Frame Fired | 定义好的动画帧开始并处理回调时触发  |
Cancel Animation Frame | 定义好的动画帧被取消  |
GC Event | 垃圾回收  |
DOMContentLoaded | DOMContentLoaded 由浏览器触发，当页面的 DOM 加载完并且解析完时触发 |
Evaluate Script | 解析脚本 |
Event | js 事件（比如 'mousedow') |
Function Call | 顶层函数调用（只会出现在进入 JS 引擎时） |
Request Animation Frame | requestAnimationFrame()调用定义好的帧 |
Remove Timer | 清除定时器 |
Time |  调用 console.time() |
Time End | 调用 console.timeEnd() |
Timer Fired | setInterval() 或 setTimeout()设定的回调 |
XHR Ready State Change | 一个异步请求就绪 |
XHR Load | 一个异步请求结束 |

##### Rendering Events
事件 | 描述 |
----|------|
Invalidate layout | DOM 改变导致页面布局失效  |
Layout | 执行页面布局计算  |
Recalculate style | 浏览器重新计算元素样式  |
Scroll | 内嵌视窗滚动 |

##### Paint Events
事件 | 描述 |
----|------|
Composite Layers | Chrome 渲染引擎完成图片复合  |
Image Decode | 图片资源被解码  |
Image Resize | 图片被修改大小 |
Paint | 复合层被绘制到对应区域。如果悬停在绘制记录上会高亮更新的区域 |

### 参考文档  

https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/timeline-tool

https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/performance-reference
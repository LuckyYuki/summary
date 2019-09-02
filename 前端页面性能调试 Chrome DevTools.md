# 前端页面性能调试 Chrome DevTools

## 内存相关问题

- **内存泄漏**：页面中的错误导致页面随着时间的延长使用的内存越来越多，页面的性能随着时间的延长越来越差。 
- **内存膨胀**：页面为达到最佳速度而使用的内存比本应使用的内存多，页面的性能一直很糟糕。
	- 内存膨胀是说占用内存太多了，但没有明确的界限，不同设备性能不同，所以要以用户为中心。了解什么设备在用户群中深受欢迎，然后在这些设备上测试页面，如果体验很差，那么页面可能存在内存膨胀的问题。
- **频繁垃圾回收**：浏览器进行垃圾回收期间，所有脚本执行都将暂停。因此，如果浏览器经常进行垃圾回收，脚本执行就会被频繁暂停或出现延迟。

### 常见内存泄漏

- 意外的全局变量
- 被遗忘的计时器
- 事件监听
- 游离 DOM 的引用
- 闭包

> 经验法则：避免对不再需要用到的DOM元素的引用，移除不需要的事件监听并且在存储你可能不会用到的大块数据时要留意。

## Chrome任务管理器

- 内存占用空间: 表示本机内存。DOM节点存储在本机内存中。如果这个值在增加，则说明正在创建DOM节点。
- JavaScript使用的内存: 表示JS堆。这一列包含两个值。关注实际使用大小即可（括号中的数字）。跳动的数字表示您网页上的可获得的对象正在使用多少内存。如果这个数字在增加，那说明正在创建新对象，或现有对象正在增长。

## Chrome Performance

### Overview 窗格

**我们可以看到 FPS，CPU，NET在页面加载时候的变化**

- FPS：每秒帧数，绿色竖线越高， FPS 越高，我们应该关注红色部分，这说明我们的页面很可能出现卡顿现象，另外 60 是一个比较理想的值。
- CPU: 和底部的 Summary 对应，显示了页面加载过程中，各阶段对 CPU 的占用时间，占用时间越多，代表该阶段越需要优化。
- NET: 每条彩色横杆代表一种资源，横杆越长，检索资源所需要的事件越长。

### Frames

Frames 这里，其实就是查看我们在什么时间，界面发生了改变，它和第一部分的区别就是在界面没有改变的时候，它是不做记录的，但是概览部分是会做记录的

### main 图表（火焰图）

main 图表展现了主线程在 Record 过程中做的所有事情，包括：Loading、Scripting、Rendering、Painting 等等。火焰图的横轴代表着时间，纵轴代表着调用堆栈。每一个长条代表执行了一个事件或函数，长条的长度代表着耗时的长短，如果某个长条右上角是红色的则表示该函数存在性能问题，需要重点关注。

在火焰图上看到几条垂直的虚线：

- 蓝线代表 DOMContentLoaded 事件
- 绿线代表首次绘制的时间
- 红线代表 load 事件

活用 Performance，按照 Chrome 的提示进行优化，可以解决掉绝大部分的性能问题。

> 展开Main图表，Devtools展示了主线程运行状况。X轴代表着时间。每个长条代表着一个event。长条越长就代表这个event花费的时间越长。Y轴代表了调用栈（call stack）。在栈里，上面的event调用了下面的event。
> 
> 在事件长条的右上角出，如果出现了红色小三角，说明这个事件是存在问题的，需要特别注意。

### CPU图表与Summary面板

在FPS图表下方，你会看到CPU图表。在CPU图表中的各种颜色与Summary面板里的颜色是相互对应的，Summary面板就在Performance面板的下方。CPU图表中的各种颜色代表着在这个时间段内，CPU在各种处理上所花费的时间。如果你看到了某个处理占用了大量的时间，那么这可能就是一个可以找到性能瓶颈的线索。

> 注意Summary面板，有时发现CPU花费了大量的时间在rendering上。因为提高性能就是一门做减法的艺术，此时目标就是减少rendering的时间

#### Performance Summary 中颜色含义

- 蓝色(Loading)：网络通信和HTML解析
- 黄色(Scripting)：JavaScript执行
- 紫色(Rendering)：样式计算和布局，即重排
- 绿色(Painting)：重绘
- 灰色(other)：其它事件花费的时间
- 白色(Idle)：空闲时间

## Chrome Memory

Heap Profiling可以记录当前的堆内存（heap）快照，并生成对象的描述文件，该描述文件给出了当时JS运行所用到的所有对象，以及这些对象所占用的内存大小、引用的层级关系等等。这些描述文件为内存泄漏的排查提供了非常有用的信息。

- Summary 总览视图：按构造函数分组。用于捕捉对象及其使用的内存。对于定位DOM内存泄露特别有用。
- Comparison 对比视图：对比两个快照。用于对比不同操作之后的堆快照，查看内存的释放及引用计数，来分析内存是否泄露及其原因。
- Containment 内容视图：查看堆内容。更适合查看对象结构，有助于分析对象的引用情况。适用于分析闭包以及深入分析对象。
- Statistics 统计视图：总览堆的统计信息。

## Performance monitor

Chrome 提供了 Performance monitor 功能，以实时直观的数据展示页面性能。

打开 Performance monitor：

- 打开 Chrome 开发者工具
- 按“Esc”，打开附加面板
- 点击选项按钮，打开选项菜单
- 选择“Performance monitor”

由于 Performance monitor 是实时的，所以，进入面板后，Performance monitor 将会自动运行，记录页面性能数据，通过点击左侧的选项，可以调整记录的数据类型。

相比 Performance，Performance monitor 的功能虽然不够全面，但胜在简洁、实时。通常情况下，可以用来分析页面使用过程中的性能问题，例如：动画性能等。

## Lighthouse

著名的开源自动化分析插件——Lighthouse

Lighthouse 不仅能够分析页面性能，还能够对 PWA、无障碍访问、SEO 等进行测试评分，并给出优化建议。

在 Chrome 60 版本之后，Chrome 开发团队直接将其加入 Chrome 开发者工具中的 Audits 面板中。

## Performance API

之前我们一直说的是基于 Chrome 浏览器的性能监测方案，但是，其实还有一种不基于浏览器的性能监测方案：**编程式性能监测**。

编程式性能监测主要依托于 W3C 推出的 Performance API，该套 API 的目的是简化开发者对网站性能进行精确分析与控制的过程，方便开发者采取手段提高 web 性能。

相比之前的性能监测方法，Performance API 最大的优点是：灵活、精确。

比如，Vue 中便封装了 Performance API 方便开发者进行性能追踪。

## 关于垃圾回收机制

### 引用

垃圾回收算法主要依赖于引用的概念。**在内存管理的环境中，一个对象如果有访问另一个对象的权限（隐式或者显式），叫做一个对象引用另一个对象。**例如：一个JavaScript对象具有对它原型的引用（隐式引用）和对它属性的引用（显式引用）。

JS相关的GC算法主要是**引用计数**（IE的BOM、DOM对象）和 **标记清除**（主流做法）

各有优劣：

- 引用计数回收及时（引用数为0立即释放掉），但循环引用就永远无法释放
- 标记清除不存在循环引用的问题（不可访问就回收掉），但回收不及时需要Stop-The-World

### Mark-and-sweep 标记-清除算法

大部分垃圾回收语言用的算法称之为 Mark-and-sweep 。

**垃圾回收器将定期从根开始，找所有从根开始引用的对象，然后找这些对象引用的对象。从根开始，垃圾回收器将找到所有可以获得的对象和收集所有不能获得的对象。**

算法由以下几步组成：

- 垃圾回收器创建了一个“roots”列表。Roots 通常是代码中全局变量的引用。JavaScript 中，“window” 对象是一个全局变量，被当作 root 。window 对象总是存在，因此垃圾回收器可以检查它和它的所有子对象是否存在（即不是垃圾）；

- 所有的 roots 被检查和标记为激活（即不是垃圾）。所有的子对象也被递归地检查。从 root 开始的所有对象如果是可达的，它就不被当作垃圾。

- 所有未被标记的内存会被当做垃圾，收集器现在可以释放内存，归还给操作系统了。

现代的垃圾回收器改良了算法，但是本质是相同的：可达内存被标记，其余的被当作垃圾回收。

### Stop-The-World 频繁GC问题

如果你的页面垃圾回收很频繁，那说明你的页面可能内存使用分配太频繁了。

在垃圾回收过程中经常涉及到对对象的挪动，进而导致需要对对象引用进行更新。为了保证引用更新的正确性，Java将暂停所有其他的线程，这种情况被称为“Stop-The-World”，导致系统全局停顿。

**Stop-The-World对系统性能存在影响，因此垃圾回收的一个原则是尽量减少“Stop-The-World”的时间。**

频繁GC很影响体验（页面暂停的感觉，因为Stop-The-World），可以通过Task Manager内存大小数值或者Performance趋势折线来看：

- Task Manager中如果内存或JS使用的内存数值频繁上升下降，就表示频繁GC
- 趋势折线中，如果JS堆大小或者节点数量频繁上升下降，表示存在频繁GC

> 可以通过优化存储结构（避免造大量的细粒度小对象）、缓存复用（比如用享元工厂来实现复用）等方式来解决频繁GC问题

## 关于动画：在 10 毫秒内生成一帧

动画不只是奇特的 UI 效果。例如，滚动和触摸拖动就是动画类型。
如果动画帧率发生变化，用户确实会注意到，我们的目标就是每秒生成 60 帧，每一帧必须完成以下所有步骤：

> JavaScript -> Style -> Layout -> Paint -> Comosite

### 60fps 与设备刷新率

目前大多数设备的屏幕刷新率为 60 次/秒。因此，**如果在页面中有一个动画或渐变效果，或者用户正在滚动页面，那么浏览器渲染动画或页面的每一帧的速率也需要跟设备屏幕的刷新率保持一致。**

其中每个帧的预算时间仅比 16 毫秒多一点 (1 秒/ 60 = 16.66 毫秒)。但实际上，浏览器有整理工作要做，因此您的所有工作需要在 10 毫秒内完成。**如果执行代码时间超过10毫秒，帧率将下降，并且内容会在屏幕上抖动。 此现象通常称为卡顿，会对用户体验产生负面影响。**

### JS动画最佳实现——requestAnimationFrame

requestAnimationFrame 比起 setTimeout、setInterval的优势主要有两点：

- requestAnimationFrame 会把每一帧中的所有DOM操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率，一般来说，这个频率为每秒60帧。
- 在隐藏或不可见的元素中，requestAnimationFrame将不会进行重绘或回流，这当然就意味着更少的的cpu，gpu和内存使用量。

## 样式、重排、重绘

### 样式

样式更改开销较大，在这些更改会影响 DOM 中的多个元素时更是如此。**只要将样式应用到元素，浏览器就必须确定对所有相关元素的影响、重新计算布局并重新绘制**。

要降低 Recalculate Style 事件的影响，使用一些对渲染性能的影响较小的属性。

>例如使用 transform 和 opacity 属性更改来实现动画，使用 will-change 或 translateZ 提升移动的元素

下面是一些常见的CSS问题

- 大开销样式计算影响响应或动画
	- 任何会更改元素几何形状的 CSS 属性，如宽度、高度或位置；浏览器必须检查所有其他元素并重做布局。**避免会触发重排的CSS属性**
- 复杂的选择器影响响应或动画
	- 嵌套选择器强制浏览器了解与所有其他元素有关的全部内容，包括父级和子级。**尽量在CSS中引用只有一个类的元素**

### 重排

布局（或重排）是浏览器用来计算页面上所有元素的位置和大小的过程。 网页的布局模式意味着一个元素可能影响其他元素；例如body元素的宽度一般会影响其子元素的宽度以及树中各处的节点等等。这个过程对于浏览器来说可能很复杂。 一般的经验法则是，**如果在帧完成前从 DOM 请求返回几何值，将发现会出现“强制同步布局”（Forced reflow），在频繁地重复或针对较大的 DOM 树执行操作时这会成为性能的大瓶颈。**

**“布局抖动”是指反复出现强制同步布局情况。**这种情况会在 JavaScript 从 DOM 反复地写入和读取时出现，将会强制浏览器反复重新计算布局

### 重绘

绘制是填充像素的过程。这经常是渲染流程开销最大的部分。如果在任何情况下注意到页面出现卡顿现象，很有可能存在绘制问题。

合成是将页面的已绘制部分放在一起以在屏幕上显示的过程。这个过程主要有两个关键因素影响页面的性能

- 需要管理的合成器数量
- 使用的动画属性

**大多数情况下，如果坚持仅合成器的属性并避免一起绘制，性能会有极大的改进，但是需要留意过多的层计数。**

对动画使用 transform 和 opacity

- 直接是合成修改阶段使用这两个属性
- 使用 transform 和 opacity 属性的时候，这些属性必须在自己的合成层上面

**提升动画元素的层数**

```javascript
.moving-element {
  will-change: transform;
  transform: translateZ(0);
}
```

**管理图层并且避免图层过多**

- 不要把所有的元素都单独成一个层，只给有需要的元素开辟单独的层数
- 因为每一个层绘制纹理都需要上传到GPU，**在内存有限的设备上，对性能的影响可能远远超过创建层带来的任何好处。每一层的纹理都需要上传到 GPU，使 CPU 与 GPU 之间的带宽、GPU 上可用于纹理处理的内存都受到进一步限制**

## 案例

### 使用分配时间线确定 JS 堆内存泄漏

```javascript
var x = [];
function grow() {
	x.push(new Array(1000000).join('x'));
}
document.getElementById('grow').addEventListener('click', grow);
```

### 使用堆快照发现已分离 DOM 树的内存泄漏

```javascript
  var detachedNodes;
  function create() {  
    var ul = document.createElement('ul');  
    for (var i = 0; i < 3; i++) {  
      var li = document.createElement('li');  
      ul.appendChild(li);  
    }  
    detachedNodes = ul;  
  }  
  document.getElementById('createBtn').addEventListener('click', create);
```

### 解除引用游离的DOM子节点：#tree对象什么时候被GC呢？

```javascript
var select = document.querySelector;
var treeRef = select("#tree");
var leafRef = select("#leaf");
var body = select("body");

body.removeChild(treeRef);

//#tree can't be GC yet due to treeRef
treeRef = null;

//#tree can't be GC yet due to indirect
//reference from leafRef

leafRef = null;
//#NOW can be #tree GC
```

![tree](http://www.ayqy.net/cms/wordpress/wp-content/uploads/2017/08/treegc.png)

> 注意：#leaf代表了对它的父节点的引用(parentNode)它递归引用到了#tree，所以，只有当leafRef被null后，#tree代表的整个树结构才会被GC回收。

### 闭包引起的内存泄漏

```javascript
var theThing = null;
var replaceThing = function () {
  var originalThing = theThing;
  var unused = function () {
    if (originalThing)
      console.log("hi");
  };
  theThing = {
    longStr: new Array(1000000).join('*'),
    someMethod: function () {
      console.log(someMessage);
    }
  };
};
// originalThing = null;
setInterval(replaceThing, 1000);
```
> 关键：只要变量被任何一个闭包使用了，就会被添加到词法环境中，被该作用域下所有闭包共享。

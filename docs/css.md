---
sidebarDepth: 4
---
# CSS
## 样式选择器

1. 标签选择器：如`div`
2. ID选择器：如`#root`
3. class选择器：如`.container`
4. 子代选择器（即父子关系）：，如`div > p`
5. 后代选择器 （即可以是爷爷和孙子的关系）：如`div p`
6. 相邻兄弟选择器：如`div + p`， 选择紧邻着div后面的p
7. 属性选择器：如`input[type=input]`
8. 伪类选择器：如`:hover`、`:first-child`、`:nth-child()`、`:first-of-type`、
9. 通配符选择器：`*`

在HTML渲染管线的样式计算环节中会计算出DOM节点最终的样式属性，具体的优先级如下：：`!important` > `inline selector` > `id selector` > `class selector` > `tag selector` > `*` > 浏览器默认样式 > 继承样式。这里的继承样式指的是部分样式如`font-size`、`color`、`visibility`是会继承给子节点的。



## 盒模型

每个DOM元素由`content`、`padding`、`border`、`margin`几个部分组成，这也被称作盒模型。

通过修改`box-sizing`的值可以调整盒模型的内部行为：

- `content-box`（默认）：元素的`width`等于元素的`content`。
- `border-box`：元素的`width`等于元素的`content + padding + border`



## `position`

- `static`
- `relative`
- `absolue`
- `fixed`
- `sticky`



## `display`

### `block`

- 例子：`div`、`h1~h6`、`p`、`header`
- 特性：元素独占一行，默认根据父元素计算出元素的宽度。可以手动设置元素`width`和`height`

### `inline`

- 例子：`a`、`span`、`img`

- 特性：元素不独占一行。不可以手动设置元素`width`和`height`

- > ``img`有点特殊，虽然它的`display`为`inline`，但它的表现更贴近`inline-block`，比如它可以手动设置`width`和`height`属性 

### `inline-block` 

- 例子：`input`
- 特性：元素不独占一行。但可以手动设置元素`width`和`height`

### `flex`

#### Flex容器属性

``` css
.flex-container {
    display: flex;
    flex-direction: row;
    /* 主轴的方向，默认row，从左往右 */
    flex-wrap: nowrap;
    /* 是否换行，默认不换行*/
    justify-content: center;
    /* 主轴上的布局，默认flex-start */
    align-items: center;
    /* 交叉轴上的布局，默认值flex-start */
    align-content: center;
    /* 多条轴线的布局 */
}
```

#### flex子元素属性

``` css
.flex-items {
    order: 2;
    /* 项目的order， 越大的越后面*/
    flex-grow: 1;
    /* 扩张比例，默认0，不占剩余空间 */
    flex-shrink: 0;
    /* 缩小比例，默认1，自动缩小*/
    flex-basis: 200px;
    /* 主轴上的宽度 */
    flex: 1 0 200px;
    /* 上面三条的缩写 */
    align-self: flex-end;
    /* 修改项目的交叉轴布局*/
}
```

#### Flex 搭配 margin

给Flex容器内的项目设置margin为auto，则margin会自动占据剩下的所有未分配空间。

``` html
<!-- 实现一个导航栏 -->
<ul class="g-nav">
    <li>导航A</li>
    <li>导航B</li>
    <li>导航C</li>
    <li>导航D</li>
    <li class="g-login">登陆</li>
    <li>注册</li>
</ul>
<style>
    .g-nav {
        display: flex;
        padding: 0;
        margin: 0;
        list-style: none;
    }
    .g-nav li {
        padding: 0 20px;
    }

    .g-login {
        margin-left: auto;
    }
</style>
```



### `grid`

`Flex`是通用的一维布局方案，可以解决绝大多数布局问题。`Grid`是二维布局方案，在某些场景下使用可能有奇效。

我们通常使用`grid-template-rows`、`grid-template-columns`来划分网格轨道，从而把项目分割成一个个的网格，同时可以使用`grid-template-area`把多个不同网格用同一个符号标识。这三个属性的缩写为`grid-template`。对于这样的网格，我们称之为**显式网格**。

某些时候，比如当实际的网格数量大于预设的显式数量，又或者当某个网格项被放置在显式网格之外，这时候就会自动生成新的网格轨道，这就是**隐式网格**。对于隐式网格我们可以使用`grid-auto-row`、`grid-auto-column`来指定网格轨道的宽高，还可以使用`grid-auto-flow`控制网格的排序方式。

对于以上介绍的六个属性，我们可以使用`grid`进行简写。它的值有三种写法：①和`grid-template`一致，只用来表示显式网格。②`grid-template-rows / [auto-flow && dense?] grid-auto-column`。③`[auto-flow && dense?] grid-auto-row / grid-template-columns`。这里的`auto-flow`指的是`grid-auto-flow`的值，当`auto-flow`写在`/`号前面表示`grid-auto-flow: row`，当`auto-flow`写在`/`号后面表示`grid-auto-flow: column`。



## 常见布局

### 垂直水平居中

- 基于绝对定位

  ``` css
  /* case1 绝对定位 + margin偏移 */
  .outer {
      position: relative
  }
  
  .inner {
      width: 200px;
      height: 200px;
      position: absolute;
      top: 50%;
      left: 50%;
      margin-top: -100px;
      margin-left: -100px;
  }
  
  /* case2 绝对定位 + calc计算位置 */
  .outer {
      position: relative
  }
  
  .inner {
      width: 200px;
      height: 200px;
      border-radius: 50%;
      position: absolute;
      top: calc(50% - 100px);
      left: calc(50% - 100px);
  }
  
  
  /* case3 绝对定位 + transform偏移 */
  .outer {
      position: relative
  }
  
  .inner {
      width: 200px;
      height: 200px;
      border-radius: 50%;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%); /* 这个方法更常用，自身的宽高可以是未知的 */
  }
  ```

- 基于`flex`布局

  ``` css
  /* case1 flex + margin: auto */
  .outer {
      display: flex;
  }
  .inner {
      width: 200px;
      height: 200px;
      border-radius: 50%;
      margin: auto;
  }
  
  /* case2 flex + margin: auto */
  
  .outer {
      display: flex;
      justify-content: center;
      align-items: center;
  }
  ```



### 三列布局

- 基于绝对定位

  ``` css
  .outer {
      height: 100px;
      position: relative;
  }
  
  .left {
    position: absolute;
    width: 100px;
    height: 100%;
    background: red;
  }
  
  .right {
    position: absolute;
    top: 0;
    right: 0;
    width: 200px;
    height: 100%;
    background: pink;
  }
  
  .center {
    margin-left: 100px;
    margin-right: 200px;
    height: 100%;
    background: skyblue;
  }
  ```

- 基于`flex`布局

  ``` css
  .outer {
      height: 100px;
      display: flex;
  }
  
  .left {
      flex-basis: 100px;
      background-color: aquamarine;
      order: 1;
  }
  
  .right {
      flex-basis: 200px;
      background-color: pink;
      order: 3;
  }
  
  .center {
      flex: 1;
      background-color: bisque;
      order: 2;
  }
  ```

## `transition`

``` css
.app {
    transition-property: width;
    transition-duration: 3s;
    transition-timing-function: ease-in;
    transition-delay: 1s;
}
```



## `animation`

``` css
@keyframes anime {
    from {
        background: pink;
    }

    to {
        background: yellow;
    }
}

.app {
    <!-- 动画名 -->
    animation-name: anime;
    <!-- 动画持续时间 -->
    animation-duration: 3s;
    <!-- 动画曲线--> 
    animation-timing-function: ease-in-out;
    <!-- 延迟 --> 
    animation-delay: 1s;
    <!-- 动画播放次数--> 
    animation-iteration-count: 2;
    <!-- 动画是否在下一周期逆向地播放--> 
    animation-direction: alternate;
    <!-- 动画是在运行还是暂停--> 
    animation-play-state: paused;
    <!-- 动画的结束状态--> 
    animation-fill-mode: forwards;
}

```



## 文本换行

### [word-break](https://developer.mozilla.org/en-US/docs/Web/CSS/word-break)

默认情况下，当元素内的文本长度超出了父元素的宽度，就有可能发生文本换行。而浏览器针对不同语言的文本也采取了不同的策略，对于CJK文本（即中文、日本、韩文）来说默认会在两个字符间换行，而对于非CJK文本（比如英文）只能在空白符处换行。

通过修改`word-break`属性可以调整文本的预期换行行为：

- `normal`：默认行为，即中日韩文本可以在任意两个字符间换行，而非中日韩字符只能在空白符处换行。
- `break-all`：所有文本都在任意两个字符间换行。
- `keep-all`：所有文本都在在空白符处换行。
- `break-word`：可以理解为比`break-all`更进了一步，在任意两个词组间换行。



### 单行文本溢出

``` css
white-space: nowrap; /* 文本超出时不换行 */
overflow: hidden; /* 溢出宽度时隐藏 */
text-overflow: ellipsis; /* 溢出时用省略号代替被隐藏的文本 */
```



### 多行文本溢出

``` css
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
display: box;
-webkit-line-clamp: 3;
-webkit-box-orient: vertical;
```





## `object-fit`

对于`img`标签这种可替换元素来说，当我们不指定元素的宽高，元素的宽高将根据原图的宽高计算得出；当我们只指定元素的`width`，会根据原图的宽高比例计算出`height`；如果我们指定了元素的`width`和`height`但比例和原图比例不一致时，图片就会自适应的进行拉伸，此时`object-fit`默认值为`fill`

我们可以通过修改`object-fit`来调整图片适应宽高的策略。

- `object-fit: fill` 填充，因此图片会被拉伸或压缩
- `object-fit: cover`填充，同时保证图片比例不变，因此图片会被裁剪
- `object-fit: contain`容纳图片，因此元素两侧会出现空白区域



## `background`

### `background-size`

- `cover`
- `contain`
- 指定具体宽高

### `background-origin`

- `border-box`
- `padding-box`（默认）：背景从`padding`开始绘制
- `content-box`

### `background-clip`

表示图片裁剪范围，默认值是`border-box`，有意思的是我们可以把它的值设置为`text`，即把背景裁剪到字体身上。

``` html
<style media="screen">
    .gradient-text {
        background: -webkit-linear-gradient(pink, red);
        -webkit-background-clip: text;
        color: transparent;
    }
</style>
<p class="gradient-text">Gradient text</p>
```





## 常见面试问题

### `display: none`和`visibility: hidden`的区别

面试经常出现的一个问题，核心点在于`display: none`的DOM元素在布局阶段中会被擦除（即布局树上不存在对应节点）。

这里我们再额外拓展下，补充上`visibility: hidden`和`opacity: 0`的区别。

- 结构上

  `display: none` ：布局树中不存在对应节点，因此**不占布局空间，而且不能点击。**

  `visibility: hidden`：布局树中存在对应节点，因此**占据布局空间，但是不能点击。**

  `opacity: 0`：布局树中存在对应节点，因此**占空间，而且能响应点击事件。**

- 继承上

  `display: none` ：作用于父元素后，子元素也不会被渲染（即使给子元素加了`display: block`）

  `visibility: hidden`：作用于父元素后，子元素继承这个属性，也不可见；不过可以给子元素设置`visibility: visible`使其可见。

  `opacity: 0`：作用于父元素后，虽然子元素不会继承这个属性，但是子元素的透明度也会被影响，所以也不可见；而且不能通过给子元素设置`opacity: 1`使其变成不透明。

- 性能上

  `display: none`：会造成回流/重绘，性能影响大

  `visibility: hidden`：会造成元素内部的重绘，性能影响相对小

  `opacity: 0` ：由于`opacity`属性启用了GPU加速，性能最好

> `opacity`是不继承属性，父元素设置opacity，子元素并不会继承。但是因为该属性的特殊性（类似`background`），父元素有了透明度，子元素的样式也会被影响。如果父元素设置`opacity: 0.5`，子元素设置`opacity: 0.5`，那么实际上子元素的透明度是0.5 * 0.5 = 0.25。
>
> 
>
> 如果希望子元素不被父元素的透明度影响，我们可以使用`background: rgba`代替`opacity: 0`

### 判断蓝色区域的宽度和高度

``` html
<html>
  <head>
    <style>
      .box {
        width: 10px;
        height: 10px;
        border: 1px solid red;
        margin: 2px;
        padding: 2px;
        background: blue;
      }

      #borderBox {
        box-sizing: border-box;
      }

      #contentBox {
        box-sizing: content-box;
      }
    </style>
  </head>
  <body>
    <div>请问下面两个 div 元素，蓝色区域的宽高各是多少像素？</div>
    <div id="borderBox" class="box"></div>
    <div id="contentBox" class="box"></div>
  </body>
</html>
```

> 主要考察盒模型（box-sizing）和background-origin默认值

答案：

- borderBox：10px(width) - 1px(border) * 2 = 8px 
- contentBox：10px(width) + 2px(padding) *2 = 14px



### Block Formatting Contexts（BFC）

在浏览器渲染流程章节中我们介绍了布局节点，提到了这涉及了许多复杂的算法，其中有一个影响布局计算的因素就是一个叫做块级格式化上下文（Block Formatting Contexts）的概念。

下面列举部分会创建出BFC的场景：

- 根元素`<html>`
- 浮动元素，即`float`的值不为`none`。
- 绝对定位元素，即`position`的值为`absolute`或`fixed`。
- `inline-block`
- `flex/grid`的子元素（前提是元素本身不是另一个`flex/grid`的容器）
- `overflow`的值为`auto`、`scroll`或`hidden`。



通过在根元素`<html>`这个BFC内部创建出新的BFC，我们的BFC将不受到外层BFC浮动`flow`的影响，而且我们的BFC包含了自身内部的浮动`flow`，还能够用来避免外边距合并（同属一个BFC的相邻元素会发生外边距margin合并）

- 清除浮动（可用来阻止元素高度塌陷）

``` html
<style>
    .outer {
        <!-- 使用overflow: auto;使outer元素成为BFC（触发outer元素的BFC）-->
        overflow: auto;
    }
    .inner {
        width: 200px;
        height: 200px;
        float: left;
    }
</style>
<body>
    <div class='outer'>
        <div class='inner'>
            
        </div>
    </div>
</body>
```

- 外边距合并

```html
<style>
    .upper {
        margin: 20px;
    }
    .lower {
        margin: 20px;
    }
    .bfc {
        overflow: auto;
    }
</style>
<div class="upper"></div>
<div class="bfc">
    <div class="lower">

    </div>
</div>
```

### 清除浮动

- 通过上文提到的创建BFC来清除浮动

- 添加额外标签，应用`clear: both`

``` html
<style>
    .float {
        float: left;
    }
    .clear {
        clear: both;
    }
</style>
<div>
    <div class="float">

    </div>
    <div class="clear">

    </div>
</div>
```

- 使用伪元素，应用`clear: both`

``` html
<style>
    .float {
        float: left;
    }
    .clearfix:after {
        content: "";
        display: block;
        clear: both;
    }
</style>
<div class="clearfix">
    <div class="float">

    </div>
</div>
```



### 行内元素的间隙

两个`inline`或`inline-block`元素中会产生间隙。

``` html
<div>
    <span>item</span> 
    <span>item</span>
</div>
```

可以看作是`<span>item item</span>`

解决方案:

1. 将子元素标签的结束符和下一个标签的开始符写在同一行或把所有子标签写在同一行

   ``` html
   <body>
       <div class="a">
           1
       </div><div class="a">
           2
       </div>
   </body>
   ```

2. 父元素设置font-size: 0; 子元素重新设置正确的font-size





## CSS开发范式

### style

``` css
.test {
    color: red;
}
```

``` js
import './test.css'

function App() {
    return <div className="test"></div>
}
```



### CSS Modules

``` css
.test {
    color: red;
}
```

``` js
import styles from './test.css'
function App() {
    return <div className={styles.test}></div>
}
```

使用`CSS Module`时有几个需要注意的点

1. 通常会对类名进行转化，如定义了`.test`时，编译后的类名可能为`.test_index-xxx`

2. 为了使某些类名不被进行转化，可以使用`:global`，如

   ``` css
   .test {
       color: red;
   }
   
   :global {
       .test2 {
           background: blue;
       }
   }
   ```

   ``` js
   import styles from './test.css'
   function App() {
       return <>
           <div className={styles.test}></div>
           <div className="test2"></div>
       </>
   }
   ```

   

### CSS-in-JS

以`emotion`库举例。

``` js
import { css } from 'emotion'
function App() {
    return <div className={css({
        color: 'red'
    })}></div>
}
```







## 移动端布局碎碎念

首先有个现象需要知道。在PC浏览器中，当内容的宽大于`viewport`的宽时，我们可以看到横向的滚动条；而在手机浏览器中表现是不同的，此时当内容的宽大于`viewport`的宽时，我们的手机屏幕依然能够显示这些内容（没有滚动条）。更加具体地说，我们知道IphoneX的像素宽为`375px`，无论我们的`html`有没有加`meta`头部，只要`html`内容宽度小于某个较大的数值，整个手机屏幕都可以放下内容（没有滚动条）；只有当`html`内容宽度大于那个数值，我们才能够看到滚动条。这个现象可以自行验证。



如果我们写了一个不带`meta`头部的`html`页面，并在手机浏览器打开，可能会觉得页面呈现出的效果完全不符合预期，甚至可以说得上诡异。

我们现在开发手机页面基本上必须带上`meta`头部，那为什么不带这个头部时，浏览器表现的这么奇怪呢？

``` html
<meta name="viewport" content="width=device-width,initial-scale=1">
```

这是有历史原因的，我们知道，智能手机的诞生要远远晚于PC浏览器。而智能手机诞生后，为了能更方便的浏览当时的页面（当时页面普遍宽980px左右），手机浏览器的默认`viewport`也被设置成了980px。`viewport`980px意味着在小小的手机屏幕上放置了过量的内容，所以那个时候我们需要使用双指缩放整个页面，然后滑动手指来阅读页面。

因此，现代的移动端页面都应该带上`meta`，根据当前的移动设备来设置`viewport`，比如手机是IphoneX的话，`viewport`就会被设置为375px。





想要开发一个现代化的、用户体验良好的网站，最重要的就是满足以下设备浏览器的适配：①PC，②Ipad，③手机。

我们可以通过PC浏览器访问知名的站点，比如B站、知乎、Github、V2ex等，再不断调整浏览器`viewport`，从而观察这些网站是如何适配不同的设备的。

##### PC -> Ipad

通过观察上述的几个站点首页，能够发现他们存在一个相似点：页面都存在**留白区域**，并且基本上页面主体内容在中间，左右留白。而这些留白区域主要是依靠`margin: 0 auto`或`flex`等方法来实现的。

当我们通过调整浏览器的可视区域来缩小`viewport`，比如从最初的`1920px`缩短至`1030px`左右（后者的值接近Ipad的`viewport`，数值上下略有浮动），这些留白部分也随之变少，直到消失。

而我们的主体内容几乎没有变化，从而实现了**PC端向Ipad端的适配**。

> 像知乎，腾讯课堂。当viewport缩放到1030px左右，主体内容已经被遮挡住一些了，所以无法直接适配Ipad。当我们用Ipad访问这两个网站时，可以很清楚的发现**页面重置**了一下，大概是开发者修改了Ipad的viewport从而容纳更多的内容

##### Ipad -> Phone

当我们继续调整浏览器的可视区域，从`1030px`左右缩短至`400px`左右（大部分手机的`viewport`在这个值附近浮动），**理论上可以直接实现Ipad端向移动端的适配**。

但这有个前提是我们手机`400px`的`viewport`可以容纳原本`1030px`乃至`1920px`才放得下的内容，`1920px`可以通过减少留白区域这种取巧的方式来实现向`1030px`的适配，但`1030px`已经填满了内容，很难再继续直接适配`400px`了。

我们用Ipad打开B站，它的首页可以容纳三十张左右的图片，但我们不可能在手机上放下这么多的内容——那用户体验也太差了。

因此B站实际上分别为了PC和移动端维护了一份代码。当我们访问`bilibili.com`时，服务器根据我们的请求头来识别这个请求是来自PC浏览器还是手机浏览器。如果是手机浏览器，它就会使我们跳转到`m.bilibili.com`，从而给我们移动端的页面。

> 当我们将`viewport`从`1030px`左右逐渐缩短，页面会出现横向的滚动条，这是因为B站的PC页面设置了`min-width`。



B站无法直接实现Ipad向移动端的适配，主要原因是页面的内容太多，特别是有许多图片。

除了额外写一套代码来适配移动端，对于没有太多内容的网站来说，可以借助媒体查询和响应式来实现**Ipad端向移动端的适配**。具体的例子有Github，Vuepress、Firefox等，更多其他的媒体查询例子可以在[该网站](https://mediaqueri.es/)找到。



##### vw 和 rem

1. `1em`等于一倍父元素的字体的大小
2. `1rem`等于一倍根元素（`html`标签）的字体的大小
3. `1vw`等于`1%`的`viewport`宽
4. `1vh`等于`1%`的`viewport`高



开发移动端页面时，需要注意的是不同手机设备的`viewport`都是有差异的。所以我们通常不会给元素一个固定像素的宽高，比如`50px`这种。否则可能页面在A手机上显示正常，再B手机上又不符合预期。

所以我们需要一个相对于`viewport`的单位，也就是`vw`了。



而以前使用`rem`来写移动端主要是**历史原因**了，早年各大浏览器对`vw`的单位还远不如今天这么完美。

以前的移动端开发通常使用`rem`单位配合淘系团队的`flexible.js`使用，`flexisble.js`这个库简单来说就是根据设备的不同，为根元素设置不同的`font-size`。又因为我们使用了`rem`单位，所以元素大小就和`viewport`相关联了。

总的来说就是以后只用`vw`就行了。



##### 主流站点布局

> 通过观察各大主流网站的布局，熟悉流行的布局方案
>
> 待更新...

**B站**

头部借助`flex`实现弹性伸缩：内容小于容器（即`viewport`）时，因为使用了`space-between`，可以看到很多空白内容；内容大于容器时，使用`flex-shrink: 1`使搜索框宽度变小。

主体使用了`margin: 0 auto`；除此之外还借助媒体查询，当`viewport`越来越小，首页的内容也会动态删减，并且会逐步减小内容的宽度。

``` css
@media screen and (max-width: 1438px)
.b-wrap {
    width: 999px;
}

@media screen and (max-width: 1654px)
.b-wrap {
    width: 1198px;
}

@media screen and (max-width: 1870px)
.b-wrap {
    width: 1414px;
}

.b-wrap {
    width: 1630px;
    margin: 0 auto;
}
```



页面设置了`min-width`，因此当`viewport`小于`1030px`左右时，会出现横向的滚动条。手机端使用的是另一套代码。

**知乎**

头部使用`margin: 0 auto`，同时借助`flex`实现弹性伸缩：内容小于容器（即`viewport`）时，使用`flex-grow: 1`占据剩余空间；内容大于容器时，使用`flex-shrink: 1`使搜索框宽度变小。

主体使用定宽加上`margin: 0 auto`

**V2ex**

头部和主体都使用`margin: 0 auto`，同时主体使用`max-width`搭配`min-width`实现右边栏定宽的两列布局


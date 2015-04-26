### 使用图例

#### 简单图例与气泡悬浮框

图例是地图不可或缺的重要组成部分。CartoCSS气泡悬浮框和图例可以为地图增加交互效果、附加信息和上下文信息。我们在这一节中就来讨论一下如何为地图添加气泡悬浮框和图例。

##### 气泡悬浮框

气泡悬浮框是当用户的鼠标悬浮或点击地图上的某个要素时展示出来的动态内容。由于在其中可以包含HTML，所以它在展示其它属性数据、图片等更多详细信息时非常有用。

这里我们以一幅地震分布地图为例说明如何实现在鼠标悬浮于每个地震点时展示出震级和地震发生时间。

1. 打开Templates面板  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-6.png)  

2. 选择“Teaser”标签页。鼠标悬浮在目标对象上时展示的是**小样**（译注：Teaser原意是正式的电影或广告播放之前，放出的预览内容，但相对于正式的“预告片”，也就是trailer，又没有那么内容丰富，所以这里译作“小样”），而点击目标对象时则展示出完整信息。可以通过为“Location”字段赋予一个URL值来定义要素对象在单击时需要加载的页面。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-1.png)  

3. 选择“Earthquakes”图层用于本次交互。注意一次只能选择一个用于交互的图层。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-2.png)  

4. 图层中需要显示的数据字段都要放在[Mustache](http://mustache.github.com/)括号标签中。这些标签在地图交互时会被替换为对应的值。把你想要显示的字段填入花括号标签。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-3.png)  

5. 使用Mustache标签来构造你的悬浮框模板。可以把下面的代码拷贝到小样文本框中，然后用预览功能看看效果。  
	  
	`{{{Magnitude}}} Magnitude Earthquake<br/>{{{DateTime}}}  ![](/tilemill/assets/pages/tooltips-4.png)`  

6. 点击“Save”保存并刷新地图。点击(X)按钮关闭Templates面板（或者可以直接按ESC键）。在地图上试试用鼠标悬浮在某个地震点上看看气泡悬浮框的展示效果。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-5.png)  

##### 图例

图例是始终展示在地图上，用于对地图的标题、描述、符号的含义等关键信息进行说明的制图要素。图例可以是一段HTML，也可以是一张简单的图片。

现在就让我们来为地图增加一个描述其专题含义的图例。

1. 打开“Templates”面板。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-6.png)  

2. 进入面板后默认打开的就是图例标签页。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/legend-1.png)  

3. 在图例文本框中输入以下text/html文本：  
	  
	`<strong>Magnitude 2.5+ Earthquakes (Past 7 Days)</strong><br/>Circle size indicates magnitude of earthquake.  ![](/tilemill/assets/pages/legend-2.png)`  

4. 点击“Save”保存并关闭面板。这时图例应该就已经出现在地图的右下角了。

**HTML的合法性问题**

安全起见，HTML格式的工具条和图例中的不安全代码和所有JavaScript代码都会被移除。如果你真的想利用JavaScript构建一些具有高级交互能力的地图，那么可以试试[MapBox.js API](https://www.mapbox.com/mapbox.js/api/v1.4.0/)。

#### 高级图例

When designing a legend for TileMill that requires more than plain text, there are a few paths you can take. An image, html/css, or a combination. Both have their advantages and disadvantages.

之前我们介绍了图例与气泡悬浮框的基本用法。那能否实现一些不只是简单文本的高级图例呢？可以，而且有好几种方法可以实现。高级图例可以是图片、html/css或者是这些要素的组合，这些方法各有利弊。

##### 嵌入图片

For complex graphics and those that feel more comfortable designing in a graphics editor. This involves creating a PNG or JPG and either serving it on the web and linking to it, or [base64-encoding it directly into the legend](https://www.mapbox.com/tilemill/docs/guides/images-in-tooltips/).

对于比较复杂的图形，最好是在专业的图形图像编辑中进行设计（译注：这句话的原文是没有谓语的，不是个完整的句子，所以这里是推测出来的意思）。图形图像可以是PNG或者JPG格式，既可以保存在Web服务器上也可以通过外链的方式引用，或者可以直接[在图例中使用base64编码方式嵌入](https://www.mapbox.com/tilemill/docs/guides/images-in-tooltips/)。

使用图片的好处在于你对它的设计可以控制到像素级，其复杂程度完全尽在设计者掌握。而图片的不足之处在于：它是静态的，一旦画到地图上就难以修改更新，要修改的话必须要找到图片的原始文件和专门的软件工具才能对其编辑修改（译注：例如Photoshop对应的ps文件，Illustrator对应的ai文件等）。

##### HTML/CSS

相比起来，利用代码把图例设计成表格形式就简单许多。这其中包括设计图例中要素的布局和样式，过程和设计网页很类似。

使用html/css的好处是你可以直接在支持CartoCSS的软件（比如TileMill）中编辑图例代码，然后就能立即看到效果。即使是在将地图导出到其它外部格式（比如MBTile）之后，也可以对图例进行维护。但是，图例的样式设计会受到很多局限（比如使用直角和纯色），并且常常是为了实现一个简单的设计效果而不得不写很多代码。

使用html/css写图例的另一个巨大优势是可以把图例代码在设计师之间和项目之间拷贝复用。下面有一些简单的图例模板帮你入门。但请注意这并不是html和css的入门指南。如果需要深入学习html和css，请参考[tizag.com](http://www.tizag.com/)，那里有更多更好的学习材料。

下面的代码可以直接拷贝到CartoCSS制图软件（比如TileMill）中。然后根据你自己的需要和本指南进行适当的修改。

![](https://www.mapbox.com/tilemill/assets/pages/advanced-legends-3.png)

**1. 水平排布的图例**
  
![](https://www.mapbox.com/tilemill/assets/pages/advanced-legends-2.png)

	
	<div class='my-legend'>
	<div class='legend-title'>The Title or Explanation of your Map</div>
	<div class='legend-scale'>
	  <ul class='legend-labels'>
	    <li><span style='background:#F1EEF6;'></span>0 - 20%</li>
	    <li><span style='background:#BDC9E1;'></span>40%</li>
	    <li><span style='background:#74A9CF;'></span>60%</li>
	    <li><span style='background:#2B8CBE;'></span>80%</li>
	    <li><span style='background:#045A8D;'></span>100%</li>
	  </ul>
	</div>
	<div class='legend-source'>Source: <a href="#link to source">Name of source</a></div>
	</div>
	
	<style type='text/css'>
	  .my-legend .legend-title {
	    text-align: left;
	    margin-bottom: 8px;
	    font-weight: bold;
	    font-size: 90%;
	  }
	  .my-legend .legend-scale ul {
	    margin: 0;
	    padding: 0;
	    float: left;
	    list-style: none;
	  }
	  .my-legend .legend-scale ul li {
	    display: block;
	    float: left;
	    width: 50px;
	    margin-bottom: 6px;
	    text-align: center;
	    font-size: 80%;
	    list-style: none;
	  }
	  .my-legend ul.legend-labels li span {
	    display: block;
	    float: left;
	    height: 15px;
	    width: 50px;
	  }
	  .my-legend .legend-source {
	    font-size: 70%;
	    color: #999;
	    clear: both;
	  }
	  .my-legend a {
	    color: #777;
	  }
	</style>
	


**2. 垂直排布的定性型图例**

![](https://www.mapbox.com/tilemill/assets/pages/advanced-legends-1.png)

	
	<div class='my-legend'>
	<div class='legend-title'>The Title or Explanation of your Map</div>
	<div class='legend-scale'>
	  <ul class='legend-labels'>
	    <li><span style='background:#8DD3C7;'></span>One</li>
	    <li><span style='background:#FFFFB3;'></span>Two</li>
	    <li><span style='background:#BEBADA;'></span>Three</li>
	    <li><span style='background:#FB8072;'></span>Four</li>
	    <li><span style='background:#80B1D3;'></span>etc</li>
	  </ul>
	</div>
	<div class='legend-source'>Source: <a href="#link to source">Name of source</a></div>
	</div>
	
	<style type='text/css'>
	  .my-legend .legend-title {
	    text-align: left;
	    margin-bottom: 5px;
	    font-weight: bold;
	    font-size: 90%;
	  }
	  .my-legend .legend-scale ul {
	    margin: 0;
	    margin-bottom: 5px;
	    padding: 0;
	    float: left;
	    list-style: none;
	  }
	  .my-legend .legend-scale ul li {
	    font-size: 80%;
	    list-style: none;
	    margin-left: 0;
	    line-height: 18px;
	    margin-bottom: 2px;
	  }
	  .my-legend ul.legend-labels li span {
	    display: block;
	    float: left;
	    height: 16px;
	    width: 30px;
	    margin-right: 5px;
	    margin-left: 0;
	    border: 1px solid #999;
	  }
	  .my-legend .legend-source {
	    font-size: 70%;
	    color: #999;
	    clear: both;
	  }
	  .my-legend a {
	    color: #777;
	  }
	</style>
	

##### 图例类别

图例可以被包含在一个具有自定义类别的要素中，就像上面两个例子中的`my-legend`那样。这个类别具有多个默认样式，包括`max-width`是`280`像素、`max-height`是`400`像素等。通常情况下，这个尺寸对于图例来说已经足够大了。但如果在实际显示时出现了滚动条，那么就说明这个尺寸不够大了。假设你需要修改这些默认属性，那么需要按下面的方法做：

在`<style></style>`标记中为`my-legend`增加一个选择器并在其中声明你要重新定义的新值。对于那些需要被覆盖重载的属性，最好在后面加上`!important`标签。比如说把最大宽度增加到`300`像素：

	
	.my-legend {
	  max-width: 300px !important;
	}
	

#### 参考文献

1. Mapbox, [Adding tooltips and legends](https://www.mapbox.com/tilemill/docs/crashcourse/tooltips/)
2. Mapbox, [Advanced legends](https://www.mapbox.com/tilemill/docs/guides/advanced-legends)

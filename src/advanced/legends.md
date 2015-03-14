### 使用图例

_译注：[原文1](https://www.mapbox.com/tilemill/docs/crashcourse/tooltips/)，[原文2](https://www.mapbox.com/tilemill/docs/guides/advanced-legends)_

#### 简单图例与浮动工具条

Tooltips and legends allow you to add interactivity, additional information, and context to your maps. Below we’ll walk through how to add each to your map.

浮动工具和图例可以为地图增加交互效果、附加信息和上下文信息。我们在这一节中就来讨论一下如何添加浮动工具和图例。

##### 浮动工具条（Tooltips）

Tooltips allow you to make maps interactive with dynamic content that appears when a user hovers over or clicks on a map. They can contain HTML and are useful for revealing additional data, images, and other content.

浮动工具条是当用户的鼠标悬浮或点击地图上的某个要素时展示出来的动态内容，因而使地图具有了交互能力。

Previously in the Importing data section of this guide, we created a map of earthquakes. Here we will add tooltips that reveal the magnitude, date, and time of each earthquake when users hovers over its point.

这里我们以一幅地震分布地图为例说明如何实现在鼠标悬浮于每个地震点时展示出震级和时间。

1. Open the Templates panel by clicking on the pointer button on the bottom left.  
	打开Templates面板  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-6.png)  

2. Click on the “Teaser” tab. Teaser content appears when you hover over a feature and Full content appears when you click on a feature. You can use the Location field to define a URL to be loaded when a feature is clicked.  
	选择“Teaser”标签页。鼠标悬浮在目标对象上时展示的是**小样**（译注：Teaser原意是正式的电影或广告播放之前，放出的预览内容，但相对于正式的“预告片”，也就是trailer，又没有那么内容丰富，所以这里译作“小样”），而点击目标对象时则展示出完整信息。可以通过为“Location”字段赋予一个URL值来定义要素对象在单击时需要加载的页面。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-1.png)  

3. Select the “Earthquakes” layer to use it for interaction. TileMill only supports one interactive layer at a time.  
	选择“Earthquakes”图层用于本次交互。注意一次只能选择一个用于交互的图层。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-2.png)  

4. The data fields for the layer are displayed wrapped in curly [Mustache](http://mustache.github.com/) tags. These tags will be replaced by data when you interact with the map. Locate the fields you want to use.  
	图层中需要显示的数据字段都需要被放在[Mustache](http://mustache.github.com/)括号标签中。这些标签在地图交互时会被替换为对应的值。选择你想要显示的字段。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-3.png)  

5. Write your template using the Mustache tags. Paste the following code into the Teaser field and use the preview to make sure it looks good:  
	使用Mustache标签来构造你的工具条模板。可以把下面的代码拷贝到小样文本框中，然后用预览功能看看效果。  
	  
	`{{{Magnitude}}} Magnitude Earthquake<br/>{{{DateTime}}}  ![](/tilemill/assets/pages/tooltips-4.png)`  

6. Click “Save” to save your settings and refresh the map. Close the panel by clicking the close button (X) or by pressing the ESC key. Move your mouse over some points to see the tooltips.  
	点击“Save”保存并刷新地图。点击(X)按钮关闭Templates面板（或者可以直接按ESC键）。在地图上试试用鼠标悬浮在某个地震点上看看浮动工具条的展示效果。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-5.png)  

##### 图例（Legends）

A legend is permanently on a map and is useful for displaying titles, descriptions, and keys for what is being mapped. It can be styled using HTML, or it can simply contain an image.

图例是始终展示在地图上，用于对地图中的标题、描述等关键信息进行说明的制图要素。图例可以使用HTML构建，也可以是一张简单的图片。

Let’s add a legend that describes the theme of the map.
让我们看看如何来为地图增加一个描述其配色方案的图例。

1. Open the Templates panel by clicking on the pointer button in the bottom left.  
	打开“Templates”面板。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-6.png)  

2. The Legend tab is open by default.  
	进入面板后默认打开的就是图例标签页。  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/legend-1.png)  

3. Enter your legend text/html in the Legend field:  
	在图例文本框中输入以下text/html文本：  
	  
	`<strong>Magnitude 2.5+ Earthquakes (Past 7 Days)</strong><br/>Circle size indicates magnitude of earthquake.  ![](/tilemill/assets/pages/legend-2.png)`  

4. Click “Save” and close the panel. You will now see your legend in the bottom right corner of the map.  
	点击“Save”保存并关闭面板。这时图例应该就已经出现在地图的右下角了。

**Allowed HTML**

**合法的HTML**

For security, unsafe HTML in tooltips and legends are sanitized and JavaScript code is removed. If you want to build sophisticated map interaction with JavaScript on your own website, you can write custom code using the [MapBox.js API](https://www.mapbox.com/mapbox.js/api/v1.4.0/).

安全起见，HTML格式的工具条和图例中的不安全代码和所有JavaScript代码都会被移除。如果你真的想利用JavaScript构建一些具有高级交互能力的地图，那么可以试试[MapBox.js API](https://www.mapbox.com/mapbox.js/api/v1.4.0/)。

#### 高级图例（Advanced Legends）

When designing a legend for TileMill that requires more than plain text, there are a few paths you can take. An image, html/css, or a combination. Both have their advantages and disadvantages.

要想实现一些不只是简单文本的高级图例，有几种方法可以做到。高级图例可以是图片、html/css或者是这些要素的组合。但无论哪种方法都是各有利弊。

##### 嵌入图片（Insert an image）

For complex graphics and those that feel more comfortable designing in a graphics editor. This involves creating a PNG or JPG and either serving it on the web and linking to it, or [base64-encoding it directly into the legend](https://www.mapbox.com/tilemill/docs/guides/images-in-tooltips/).

对于比较复杂的图形，最好是在专业的图形图像编辑中进行设计（译注：这句话的原文是没有谓语的，不是个完整的句子，所以这里是推测出来的意思）。图形图像可以是PNG或者JPG格式，既可以保存在Web服务器上也可以通过外链的方式引用，还可以直接[在图例中使用base64编码方式嵌入](https://www.mapbox.com/tilemill/docs/guides/images-in-tooltips/)。

The advantage with images is that you have the ability to design every pixel and they can be as complex as you want. The drawback is that the image is static once it’s in the map, and it may not be as easy to update, as you need the original file and software that can open it.

使用图片的好处在于你对它的设计可以控制到像素级，图片的复杂程度完全尽在设计者掌握。然而它的不足之处在于图片是静态的，一旦画到地图上就难以修改更新，因为通常需要图片的原始文件和专门的软件才能对其编辑修改（译注：例如Photoshop对应的ps文件，Illustrator对应的ai文件等）。

##### HTML/CSS

For simpler, table-like designs and those that feel more comfortable designing with code. This involves creating the layout and styling for the legend elements in the same way one would build a web page.

也可以简单点，利用代码把图例设计成表格形式。这其中包括设计图例中要素的布局和样式，过程和设计网页很类似。

The advantage with html/css is that you can quickly make edits to the legend directly in TileMill, and maintain the ability to manipulate the legend styling with css even after the map has been exported to MBTile format. However, you are limited to right angles and solid colors, and may have to write many lines of code to create a relatively simple design.

使用html/css的好处是你可以直接在支持CartoCSS的软件（比如TileMill）中编辑图例代码，然后可以立即看到效果。即使是在将地图导出到其它外部格式（比如MBTile）之后，也可以对图例进行维护。但是，图例的设计将会被局限于使用直角和纯色，并可能为了实现一个相对简单的设计而不得不编写很多代码。

Another big advantage with html/css is that you can easily pass the source code from project to project and person to person. Below are a couple of **basic templates** for getting started, and how you can make them your own. This is not intended to be a tutorial on html or css. If you would like to learn more about these languages, check out the great guides at [tizag.com](http://www.tizag.com/).

使用html/css还有个巨大的优势，就是可以把图例代码随处拷贝和复用。下面有一些简单的图例模板帮你入门。但请注意这并不是html和css的入门指南。如果需要深入学习html和css，请参考[tizag.com](http://www.tizag.com/)，那里有很好的学习材料。

Copy and paste the block of code directly into TileMill’s legend field. Then follow this guide to tweak the template for your own purposes.

把下面的代码可以直接拷贝到CartoCSS制图软件（比如TileMill）中。然后根据你自己的需要和本节内容进行适当的修改。

![](https://www.mapbox.com/tilemill/assets/pages/advanced-legends-3.png)

**1. Horizontal Sequential**

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
	


**2. Vertical Qualitative**

**2. 垂直排布的非数值型图例**

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
	

##### 图例类（The legend class）

TileMill legends can be contained within an element with a custom class (e.g. `my-legend`). This is why you see it included in each selector in the above style sections. This class is attributed several default styles, including a `max-width` of 280 pixels and a `max-height` of 400 pixels. Under normal circumstances this should be plenty large enough. You’ll know they’re not if you see a scrollbar in your legend. In case you ever need to change these, here’s how.

图例可以被包含在一个具有自定义类（比如`my-legend`）的要素中，就像上面两个例子中展示的那样。这个类具有多个默认样式，包括`max-width`是280像素、`max-height`是400像素。通常情况下，这个尺寸对于图例是足够的。但如果在实际显示时出现了滚动条，那么就说明这个尺寸不够大了。假设你需要修改这些默认属性，那么需要按照下面的方法做。

Inside the `<style></style>` tags add a selector for my-legend and declare the new value(s). For values that are overriding previous declarations, you will likely need to add the `!important` tag. Say you want to increase the width to 300 pixels:

在`<style></style>`标记中为my-legend增加一个选择器并在其中声明你要重新定义的新值。对于那些需要被覆盖重载的声明，最好在后面加上`!important`标签。比如说把最大宽度增加到300像素：

	
	.my-legend {
	  max-width: 300px !important;
	}
	



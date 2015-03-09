### 使用图例

_译注：[原文1](https://www.mapbox.com/tilemill/docs/crashcourse/tooltips/)，[原文2](https://www.mapbox.com/tilemill/docs/guides/advanced-legends)_

#### 简单图例与浮动工具条

Tooltips and legends allow you to add interactivity, additional information, and context to your maps. Below we’ll walk through how to add each to your map.

##### 浮动工具条（Tooltips）

Tooltips allow you to make maps interactive with dynamic content that appears when a user hovers over or clicks on a map. They can contain HTML and are useful for revealing additional data, images, and other content.

Previously in the Importing data section of this guide, we created a map of earthquakes. Here we will add tooltips that reveal the magnitude, date, and time of each earthquake when users hovers over its point.

1. Open the Templates panel by clicking on the pointer button on the bottom left.  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-6.png)  

2. Click on the “Teaser” tab. Teaser content appears when you hover over a feature and Full content appears when you click on a feature. You can use the Location field to define a URL to be loaded when a feature is clicked.  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-1.png)  

3. Select the “Earthquakes” layer to use it for interaction. TileMill only supports one interactive layer at a time.  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-2.png)  

4. The data fields for the layer are displayed wrapped in curly [Mustache](http://mustache.github.com/) tags. These tags will be replaced by data when you interact with the map. Locate the fields you want to use.  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-3.png)  

5. Write your template using the Mustache tags. Paste the following code into the Teaser field and use the preview to make sure it looks good:  
	  
	`{{{Magnitude}}} Magnitude Earthquake<br/>{{{DateTime}}}  ![](/tilemill/assets/pages/tooltips-4.png)`  

6. Click “Save” to save your settings and refresh the map. Close the panel by clicking the close button (X) or by pressing the ESC key. Move your mouse over some points to see the tooltips.  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-5.png)  

##### 图例（Legends）

A legend is permanently on a map and is useful for displaying titles, descriptions, and keys for what is being mapped. It can be styled using HTML, or it can simply contain an image.

Let’s add a legend that describes the theme of the map.

1. Open the Templates panel by clicking on the pointer button in the bottom left.  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/tooltips-6.png)  

2. The Legend tab is open by default.  
	  
	![](https://www.mapbox.com/tilemill/assets/pages/legend-1.png)  

3. Enter your legend text/html in the Legend field:  
	  
	`<strong>Magnitude 2.5+ Earthquakes (Past 7 Days)</strong><br/>Circle size indicates magnitude of earthquake.  ![](/tilemill/assets/pages/legend-2.png)`  

4. Click “Save” and close the panel. You will now see your legend in the bottom right corner of the map.

**Allowed HTML**

For security, unsafe HTML in tooltips and legends are sanitized and JavaScript code is removed. If you want to build sophisticated map interaction with JavaScript on your own website, you can write custom code using the [MapBox.js API](https://www.mapbox.com/mapbox.js/api/v1.4.0/).

#### 高级图例（Advanced Legends）

When designing a legend for TileMill that requires more than plain text, there are a few paths you can take. An image, html/css, or a combination. Both have their advantages and disadvantages.

##### 嵌入图片（Insert an image）

For complex graphics and those that feel more comfortable designing in a graphics editor. This involves creating a PNG or JPG and either serving it on the web and linking to it, or [base64-encoding it directly into the legend](https://www.mapbox.com/tilemill/docs/guides/images-in-tooltips/).

The advantage with images is that you have the ability to design every pixel and they can be as complex as you want. The drawback is that the image is static once it’s in the map, and it may not be as easy to update, as you need the original file and software that can open it.

##### HTML/CSS

For simpler, table-like designs and those that feel more comfortable designing with code. This involves creating the layout and styling for the legend elements in the same way one would build a web page.

The advantage with html/css is that you can quickly make edits to the legend directly in TileMill, and maintain the ability to manipulate the legend styling with css even after the map has been exported to MBTile format. However, you are limited to right angles and solid colors, and may have to write many lines of code to create a relatively simple design.

Another big advantage with html/css is that you can easily pass the source code from project to project and person to person. Below are a couple of **basic templates** for getting started, and how you can make them your own. This is not intended to be a tutorial on html or css. If you would like to learn more about these languages, check out the great guides at [tizag.com](http://www.tizag.com/).

Copy and paste the block of code directly into TileMill’s legend field. Then follow this guide to tweak the template for your own purposes.

![](https://www.mapbox.com/tilemill/assets/pages/advanced-legends-3.png)

**1. Horizontal Sequential**
  
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
	

##### The legend class

TileMill legends can be contained within an element with a custom class (e.g. `my-legend`). This is why you see it included in each selector in the above style sections. This class is attributed several default styles, including a `max-width` of 280 pixels and a `max-height` of 400 pixels. Under normal circumstances this should be plenty large enough. You’ll know they’re not if you see a scrollbar in your legend. In case you ever need to change these, here’s how.

Inside the `<style></style>` tags add a selector for my-legend and declare the new value(s). For values that are overriding previous declarations, you will likely need to add the `!important` tag. Say you want to increase the width to 300 pixels:

	
	.my-legend {
	  max-width: 300px !important;
	  }
	



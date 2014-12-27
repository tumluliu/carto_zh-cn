### 为面要素配置样式（Styling polygons）

_译注：[原文地址](https://www.mapbox.com/mapbox-studio/styling-polygons/)_

Polygons are areas that can be filled with a solid color or a pattern, and also given an outline.

面要素由内部区域和边界组成。其中，内部区域可以用纯色或图案进行填充。

_Tip: everything covered in the styling lines guide can also be applied to polygon layers._

_小贴士：用于配置线要素的方法也都同样适用于面要素图层。_

#### Basic Styling

The simplest polygon style is a solid color fill.

最简单的面样式是纯色填充。

![](https://cloud.githubusercontent.com/assets/126952/3908623/6d6b3c32-2305-11e4-9cff-9404f8f07bd1.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-polygons/)_

	
	#landuse[class='park'] {
	  polygon-fill: #bda;
	}
	

If you want to adjust the opacity of a polygon-fill you can use the polygon-opacity property. This is a number between 0 and 1 - 0 being fully transparent and 1 being fully opaque. With 50% opacity we can see overlapping shapes in the same layer add together to create more opaque areas.

如果需要调整填充色的透明度，那么就用`polygon-opacity`属性。它的取值范围是从0到1，0表示全透明，1表示完全不透明。如果把它设置成50%透明，那么可以得到这样一种效果（如下图）：如果当前图层中有相互重叠的面要素，那么重叠的部分的透明度会显得比其它部分更低一些。

![](https://cloud.githubusercontent.com/assets/126952/3908624/6d70d9d0-2305-11e4-92f2-abb819844509.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-polygons/)_

	
	#landuse[class='park'] {
	  polygon-fill: #bda;
	  polygon-opacity: 0.5;
	}
	

#### 用伽马属性调整缝隙（Gaps and Gamma）

![](https://cloud.githubusercontent.com/assets/126952/3908625/6d784346-2305-11e4-8755-d45d61d35583.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-polygons/)_

When you have a layer containing polygons that should fit together seamlessly, you might notice subtle gaps between them at certain scales. You can use the polygon-gamma style to help reduce this effect. Gamma takes a value between 0 and 1 - the default is 1, so try lowering it to hide the gaps. Be careful about setting it too low, though. You’ll get jagged edges and possibly even stranger artifacts.

如果一个面图层中的所有面要素之间（译注：指在原始数据层次，各个面要素对应的多边形几何形状）都是无缝衔接的，那么在制图渲染后它们之间也理应完好的拼接在一起。然而在实际的制图效果中，却往往会在地图缩放到某些级别上看到这些原本无缝的多边形之间出现一些细微的缝隙（译注：如上图中水域部分靠图片上方的那一条灰白色细缝）。这种缝隙效果可以用一个名为`polygon-gamma`的属性值来帮助消除。该属性的取值范围是0到1，默认值为1。可以尝试调小这个值来消除缝隙，但也要注意如果调的太小可能导致出现参差不齐的边缘甚至一些更加怪异的效果。

（译注：是否有必要对为何会产生这种细缝，以及为什么会通过调整Gamma值消除这些细缝的原因和机理进行一下解释？如果有必要，那么放在基础用法这一章中是否合适？是否应该放到高级技巧中去？）

![](https://cloud.githubusercontent.com/assets/126952/3908627/6d80f612-2305-11e4-80f4-6803335295c3.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-polygons/)_

	
	#water {
	  polygon-fill: #acf;
	  polygon-gamma: 0.5;
	}
	

#### 图案和纹理（Patterns and Textures）

With CartoCSS, you can easily fill areas with textures and patterns by bringing in external images. You might create the patterns yourself in image editing software, or find ready-made images from resource websites such as [Subtle Patterns](http://subtlepatterns.com/thumbnail-view/) or [Free Seamless Textures](http://freeseamlesstextures.com/).

在CartoCSS中，使用外部图片（译注：主要指来自文件系统或互联网的图片资源文件）作为图案和纹理对面要素进行填充是很容易实现的。用户可以自己用图像编辑软件制作这些图片，或者从图片资源网站（如[Subtle Patterns](http://subtlepatterns.com/thumbnail-view/)、[Free Seamless Textures](http://freeseamlesstextures.com/)等）上去获取现成的图片资源。

You can add a pattern style from any local file or web URL using the `polygon-pattern-file` style. Here is a simple diagonal stripe pattern you can use to try out - you can reference it from CartoCSS as in the snippet below.

用户可以从本地文件系统或互联网上添加图案样式资源，然后把资源文件的地址（文件路径或资源链接）赋给`polygon-pattern-file`属性。下面的例子是一个简单的斜纹图案，在CartoCSS中可以引用这个图片作为填充图案。

![](https://cloud.githubusercontent.com/assets/83384/3893834/32389e24-223e-11e4-8ec6-163fd55d6622.png)

![](https://cloud.githubusercontent.com/assets/126952/3908626/6d7970cc-2305-11e4-88b9-0219470cd157.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-polygons/)_

	
	#landuse[class='park'] {
	  polygon-pattern-file: url("pattern-stripe.png");
	}
	

In order to have patterns work when your style is uploaded to Mapbox they will need to be stored in the style project folder (this is the folder ending in .tm2 that’s created when you “Save As” a style project). If your project uses a lot of images you can create a subdirectory in the .tm2folder and include that in the URL. For example, if you create a subdirectory named images:  

（译注：这段文字为了去Mapbox化，完全使用了意译。）

为了让这些图案生效，这些外部的图片文件必须要保证自己是能够被CartoCSS解析器找到。如果图片文件来自本地文件系统，而且在CartoCSS脚本中是以相对路径的形式进行引用，那么就需要在与脚本文件相同的目录中建立对应的子目录。在图片资源文件较多的时候，这是一种值得推荐的做法，比如都放在images子目录中，然后通过相对路径引用这些图片资源：

	
	#landuse[class='park'] {
	  polygon-pattern-file: url("images/pattern-stripe.png");
	}
	

#### Background patterns

If you want to add a pattern image to the background of the whole map, you can use the `background-image` property on the `Map` object.

如果要在整个地图上应用某种背景图案，那么可以利用`Map`对象的`background-image`属性实现。

	
	Map {
	  background-image: url("pattern.png");
	}
	

Like all other properties on the Map object, background-image has a global effect - it cannot be filtered or changed depending on zoom level.

与其它`Map`对象上的属性一样，`background-image`的效果也是全局性的。这意味着它的渲染效果无法利用缩放级别过滤器进行调整。

##### 图案与填充的结合（Combining patterns & fills）

Using transparency or compositing operations it is possible to get a lot of variety out of a single pattern image.

通过对透明度与合成操作的灵活运用，可以从一种图案衍生出千变万化的填充效果。

##### 如何保证无缝（Ensuring seamlessness）

There are two types of pattern alignment: local (the default) and global (specified with `polygon-pattern-alignment: global;`).

关于填充图案的对齐方式，主要有两种：一种是局部（默认方式）模式，另一种是全局模式。对齐方式通过`polygon-pattern-alignment`属性进行配置。

When a pattern style has local alignment, that means that the pattern will be aligned separately for each polygon it is applied to. The top-left corner of the pattern image will be aligned to the top-left corner of a polygon’s bounding box.

如果采用本地对齐模式，那么图案图片将与每个面要素对象分别对齐。图案图像的左上角将与面要素外包框的左上角对齐。

When a pattern style has global alignment, pattern images are aligned to the image tile instead of the individual geometries. Thus a repeated pattern will line up across all of the polygons it is applied to. With global alignment, pattern images should not be larger than the metatile (excluding the buffer), otherwise portions of the pattern will never be shown.

如果采用全局对齐模式，图案图片将会与地图瓦片对齐，而非每个面要素对象。因而填充图案会在全部面要素上排队一样反复出现。在全局对齐模式下，填充图案的尺寸不能超过元瓦片（除缓冲区以外的部分），否则图案的一些部分会无法显示。

Another important thing to keep in mind is with globally-aligned patterns is that the pixel dimensions of the image file must multiply evenly up to the width and height of the tile, 256×256 pixels. Your pattern width or height dimentions could be 16 or 32 or 128, but should not 20 or 100 or any other number you can’t evenly divide 256 by. If you are using patterns from a resource website, you may need to resize them in an image editor to conform to this limitation.

在全局对齐模式下，还有一点需要特别注意：图案的尺寸必须要能够整除地图瓦片的尺寸。比如说地图瓦片的尺寸是256x256，那么图案的尺寸可以是16，32或者128，但不能是20，100或者其它不能整除256的数字。如果是从一些图片资源网站获取的图案，那么请根据这个约束条件用图像编辑软件对图案文件的尺寸进行修正。



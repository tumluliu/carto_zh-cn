# 1. CartoCSS概述

CartoCSS是一种语法类似CSS（Cascading Style Sheets，层叠样式表，一种对网页进行设计的样式语言）的制图样式描述语言。如果熟悉CSS的话，那么CartoCSS这种对地图进行样式设计的语言也会看起来不陌生，尽管二者所包含的要素、属性等内容和含义完全不同。

## 1.1 符号

higis中的地图渲染引擎提供了一组基本样式，基于这些基本样式可以配制出复杂的地图样式。这些基本样式被称为**符号**，每种符号都包含一系列属性。

higis中目前包含以下十种符号。每一种符号都可以用于特定的一种或几种空间数据类型：

1.	线符号（可用于线要素和面要素）
2.	面符号（可用于面要素）
3.	点符号（可用于点要素）
4.	文本符号（可用于点要素、线要素和面要素）
5.	盾标符号（可用于点要素和线要素）
6.	线模式（可用于线要素和面要素）
7.	面模式（可用于面要素）
8.	栅格符号（可用于栅格）
9.	注记符号（可用于点要素、线要素和面要素）
10.	建筑物符号（可用于面要素）

_需要注意的是，尽管面符号可以用于定制线要素的样式，但往往会出现不可预期的不理想结果，因此不推荐使用。_

对同一个图层可以同时应用多种符号来定制样式。这种用法我们称之为**多符号**。常用的多符号组合包括：线符号加面符号，点符号、文本符号、线符号加注记符号，以及线符号加线模式等。

一种符号只有在明确定义了它的样式之后才能被绘制在地图上。在每种符号的诸多属性中，除了显式赋值的属性以外，其它属性将全部被设置为默认值。例如，线符号中颜色属性的默认值为黑色，所以如果用户显式设置了线宽，那么图层中的线要素就将以用户设置的宽度被绘制成黑色。

## 1.2 多符号

对一个图层来说，它的样式可以不局限于仅使用单一的某种符号来定制。举例来说，为了在多边形的边界上获得一种柔和的光晕或阴影效果，可以定义多个半透明线符号，共同发挥作用达到渲染效果。再举一例，对点要素，可以通过定义多个文本符号将若干个属性字段以不同偏移的形式标注在点要素的周围。

通常，如果对一个图层定义了一种样式，那么这种样式就会应用于一种默认的符号。在下面的例子中，后一个样式规则就会将前一个覆盖，因为二者都应用了相同的默认符号，即线符号。

	
	#layer {
	  line-color: #C00;
	  line-width: 1;
	}
	
	#layer {
	  line-color: #0AF;
	  line-opacity: 0.5;
	  line-width: 2;
	}
	

用户可以通过显式声明的方式为一个图层增加任意数量的**新符号**。由这些新符号所定义的样式之间只要不互相冲突，那么它们都将被用于渲染该图层。为图层定义新符号使用双冒号“::”语法，与CSS3中的伪元素定义类似：

	
	#layer {
	  /* styles for the default symbolizers */
	}
	
	#layer::newsymbol {
	  /* styles for a new symbolizer named ‘newsymbol’ */
	}
	

注意上面例子中的newsymbol不是关键字。用户可以为新符号自定义名字，但是为了便于理解，最好取一些有意义的名字，例如：`#road::casing`, `#coastline::glow_inner`, `#building::shadow`。

在上一个例子中，我们可以通过再声明一个新符号来实现一个蓝色光晕效果。而正是通过增加了这个新符号的声明，使得蓝色光晕能够被叠加渲染在之前的红色轮廓线之上，而不是覆盖了前面红色线样式（如图）。

	
	#layer {
	  line-color: #C00;
	  line-width: 1;
	}
	
	#layer::glow {
	  line-color: #0AF;
	  line-opacity: 0.5;
	  line-width: 4;
	}
	

![](symbolizer-1.png)
_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/manual/carto/)_

在对所定义的符号进行渲染的时候，是按照其在样式脚本中出现的顺序进行的。所以上面例子中的新符号`::glow`（蓝色光晕线）会被绘制在之前定义的红色轮廓线之上。

具名的新符号样式也同样会有同名覆盖问题，即后定义的新符号会覆盖之前先定义的同名符号的样式设置。在下面的例子中，线的颜色最终将被渲染为绿色（RGB值为#3F6），而不是半透明黄色上叠加一层绿色效果（如图）。

	.border::highlight {
	  line-color: #FF0;
	  line-opacity: 0.5;
	}
	
	.border::highlight {
	  line-color: #3F6;
	}

![](symbolizer-2.png)
_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/manual/carto/)_

# 2. CartoCSS语言参考

CartoCSS提供了一系列用于定义地图样式的属性。本节给出这些属性的含义和所有可取的值。

## 2.1 所有符号的公共属性

##### image-filters `functions`

默认值： none _(不使用图像过滤器)_

说明：以函数形式提供的一组图像过滤器。图像过滤器会作用于处于活动状态的画布。如果设置了多个图像过滤器，那么每增加一个过滤器都会触发创建一个新的画布，当这个新的画布被渲染完成后，再通过合成的方式与主画布合并。如果要直接在主画布上应用图像过滤器，那么需要使用`direct-image-filters`属性。

##### direct-image-filters `functions`

默认值： none _(不使用图像过滤器)_

说明：作用于主画布上的图像过滤器（参见`image-filters`）

##### comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(把当前图层覆盖在其它图层之上)_

说明：合成操作。该属性用于定义当前图层与其相邻图层如何合成。关于合成操作，请参见[图像合成（image compositing）技术](https://www.mapbox.com/mapbox-studio/compositing-reference/)的相关内容。

##### opacity `float`

取值范围：0到1

默认值： 1 _(不透明)_

说明：设置透明度。为样式设置alpha值（首先，创建一个独立的缓冲区，在这个缓冲区中为所有要素应用alpha实现透明化，然后再把这个独立缓冲区合成到主缓冲区中）

## 2.2 地图（map）的属性

与其它十种符号不同，本节列出的是用于配置地图整体样式的属性。

##### background-color `color`

默认值： none _(透明色)_

说明：设置地图的背景颜色。

##### background-image `uri`

默认值：_(透明色)_

说明：设置一张以平铺形式置于最底层的背景图片。

##### background-image-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(背景图片被置于所设置的背景色上一层)_

说明：设置背景图片与背景颜色之间的合成操作方式。

##### background-image-opacity `float`

默认值： 1 _(背景图片的透明度保持不变)_

说明：设置背景图片的透明度。

##### srs `string`

默认值： +proj=longlat +ellps=WGS84 +datum=WGS84 +no\_defs _(这是EPSG:4326空间参考系的proj4表达形式。地图中所有图层的数据都会采用这同一种参考系来绘制。如果地图中的某一图层没有显式声明自己所使用的参考系，那么这个图层将被认为与地图的参考系相同，并且在绘制的时候不会对这个图层中包含的数据进行座标变换。)_

说明：设置地图的空间参考系（以proj4字符串表达）。

##### buffer-size `float`

默认值： 0 _(无缓冲区)_

说明：在地图周围增加一圈额外的绘制区域（以像素数表达）。这个属性的设置是为了保证那些出现在地图边界附近的文本标注不至于在渲染时被截断。注意这个属性不应该与`avoid-edges`同时使用。

##### base `string`

默认值：  _(工作路径默认为空。此时在样式定义中所有通过相对路径的方式引用的外部资源文件都会以应用程序所在的路径为父目录去寻址。)_

说明：如果map是从内存中加载的，那么所有以相对路径方式引用的外部资源则均为相对于由该base属性定义的路径。如果map是从文件系统中加载的，并且这个base属性没有被显式设置，那么base的值就是样式文件所在的目录。

##### font-directory `uri`

默认值： none _(不注册专门用于当前map的字体)_

说明：指定专门用于当前map的字体目录（自动加载的默认字体除外）

## 2.3 线符号（line）的属性

##### line-color `color`

默认值： rgba(0,0,0,1) _(完全不透明的黑色)_

说明：设置线要素的线条颜色。

##### line-width `float`

默认值： 1

说明：设置线要素的线条宽度，单位为像素。

##### line-opacity `float`

默认值： 1 _(不透明)_

说明：设置线要素的透明度。

##### line-join `keyword`

取值范围：`miter` `round` `bevel`

默认值： miter

说明：设置线要素之间在交汇点处如何绘制。三种绘制方法的比较如下图所示。

![](linejoin.png)
_图片来源：[w3.org](http://www.w3.org/TR/SVG/painting.html "line join")_

##### line-cap `keyword`

取值范围：`butt` `round` `square`

默认值： butt

说明：设置线要素的端点形状。

##### line-gamma `float`

取值范围：0到1

默认值： 1 _(完全抗锯齿)_

说明：设置绘制线要素时的抗锯齿级别。

##### line-gamma-method `keyword`

取值范围：`power` `linear` `none` `threshold` `multiply`

默认值： power _(使用pow(x, gamma)来计算像素gamma值。与linear相比，应用power值绘制出来的线与面要素更加平滑。而其它的取值通常只是用来关闭抗锯齿。)_

说明：设置抗锯齿的具体算法，控制绘制质量。

##### line-dasharray `numbers`

默认值： none _(实线（无虚线效果）)_

说明：通过设置[a,b]的值设置虚线样式。其中a为虚线段的长度，b为虚线段间隔的长度。此外，还可以设置更多的值，从而获得更加复杂的绘制效果。

##### line-miterlimit `float`

默认值： 4 _(当theta角小于29度时，自动将线要素交汇样式从miter改为bevel)_

说明：设置线端的切角长度与线宽的比例上限。当线要素交汇处出现尖锐的锐角时，由于切角与线宽比例失调会导致错误的绘制结果。设置了该属性则会在出现上述情况时自动将线要素交汇样式从miter改为bevel。一般情况下，这个属性不需要显式设置，但有时可以通过设定一个较大的值可以避免出现参差不齐的不良绘制效果。

##### line-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。

##### line-simplify `float`

默认值： 0 _(不对几何要素进行简化)_

说明：如果要对几何要素按照地图综合的方法进行简化，那么通过该属性来设定阈值。

##### line-simplify-algorithm `keyword`

取值范围：`radial-distance` `zhao-saalfeld` `visvalingam-whyatt`

默认值： radial-distance _(不使用radial-distance算法进行简化)_

说明：设置对线要素进行综合的简化算法。

##### line-smooth `float`

取值范围：0到1

默认值： 0 _(不对拐点进行平滑)_

说明：对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。

##### line-offset `float`

默认值： 0 _(无偏移)_

说明：将线要素相对于其原有位置向左（沿着线的走向）或向右偏移一定量的像素绘制。正值表示左偏，负值表示右偏。

##### line-rasterizer `keyword`

取值范围：`full` `fast`

默认值： full

说明：设置线渲染方式，可以通过牺牲部分精确度以换取绘制速度。

##### line-geometry-transform `functions`

默认值： none _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。

##### line-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

## 2.4 面符号（polygon）的属性

##### polygon-fill `color`

默认值： rgba(128,128,128,1) _(完全不透明的灰色)_

说明：设置面要素的填充色

##### polygon-opacity `float`

默认值： 1 _(不透明)_

说明：面要素的透明度（0为完全透明，1为完全不透明）

##### polygon-gamma `float`

取值范围：0到1

默认值： 1 _(完全抗锯齿)_

说明：设置面要素边缘的抗锯齿级别，影响绘制效果和速度。抗锯齿级别越高（最高为1），绘制效果越好，但绘制速度最慢；反之，绘制效果最差，但绘制速度最快。注意这里所说的绘制速度的快慢只是理论上的，实际效果与软硬件环境密切相关。

##### polygon-gamma-method `keyword`

取值范围：`power` `linear` `none` `threshold` `multiply`

默认值： power _(使用pow(x, gamma)来计算像素gamma值。与linear相比，应用power值绘制出来的线与面要素更加平滑。而其它的取值通常只是用来关闭抗锯齿。)_

说明：设置抗锯齿的具体算法，控制绘制质量。

##### polygon-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。

##### polygon-simplify `float`

默认值： 0 _(不对面要素的边线进行简化)_

说明：如果要对面要素的边线按照地图综合的方法进行简化，那么通过该属性来设定阈值。（参见地图综合中的线简化算法，如Douglas-Peuker算法）

##### polygon-simplify-algorithm `keyword`

取值范围：`radial-distance` `zhao-saalfeld` `visvalingam-whyatt`

默认值： radial-distance _(不使用radial-distance算法进行简化)_

说明：设置对面要素的边线进行综合的简化算法。

##### polygon-smooth `float`

取值范围：0到1

默认值： 0 _(不对拐点进行平滑)_

说明：对边线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。

##### polygon-geometry-transform `functions`

默认值： none _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。

##### polygon-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

## 2.5 点符号（point）的属性

##### point-file `uri`

默认值： none

说明：设置用于绘制点符号的图像文件。

##### point-allow-overlap `boolean`

默认值： false _(不允许点符号相互压盖)_

说明：设置是否显式相互压盖的点符号。

##### point-ignore-placement `boolean`

默认值： false _(不在冲突检测器的缓存中存储几何形状的外包框)_

说明：设置是否允许在与当前要素重叠的位置放置其它要素。

##### point-opacity `float`

取值范围：0到1

默认值： 1 _(完全不透明)_

说明：设置点符号的透明度。

##### point-placement `keyword`

取值范围：`centroid` `interior`

默认值： centroid

说明：设置点符号的放置方式。

##### point-transform `functions`

默认值： _(无变换)_

说明：设置SVG图形的变换方法。

##### point-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

## 2.6 文本符号（text）的属性

##### text-name `expression`

默认值： 

说明：设置文本符号上显示的文字。可以通过用中括号括起来的字段名来指定要使用的数据字段，例如[column\_name]。

##### text-face-name `string`

默认值： undefined

说明：设置文本符号所使用的字体。

##### text-size `float`

默认值： 10

说明：设置文本符号中文字的字号，以像素为单位。

##### text-ratio `unsigned`

默认值： 0

说明：设置折行后的文本所占比例。

##### text-wrap-width `unsigned`

默认值： 0

说明：设置文本块在多长的时候进行折行，以字符为单位。

##### text-wrap-before `boolean`

默认值： false

说明：控制文本文字的折行动作。如果该值为false，那么每一行文本都会比wrap-width属性设定的值略长。

##### text-wrap-character `string`

默认值：  

说明：设置盾标文本的折行字符。

##### text-spacing `unsigned`

默认值： undefined

说明：设置沿线绘制文本符号时每两个文本符号之间的间距。

##### text-character-spacing `float`

默认值： 0

说明：设置文本文字的字间距，以像素为单位。

##### text-line-spacing `unsigned`

默认值： 0

说明：设置文本文字的行间距。

##### text-label-position-tolerance `unsigned`

默认值： 0

说明：设置文本标注相对于其理想位置的偏移量，以像素为单位（目前仅适用于线要素）

##### text-max-char-angle-delta `float`

默认值： 22.5

说明：设置文本文字的最大折转角，以十进制角度为单位。这个值会在绘制时被换算成弧度，例如默认的22.5度会按照22.5/math.pi\*180.0公式被换算成0.3925弧度。这个值越大，则被绘制在尖锐转角处的文本符号会越少。

##### text-fill `color`

默认值： #000000 _(黑色)_

说明：设置文本文字的颜色。

##### text-opacity `float`

取值范围：0到1

默认值： 1 _(不透明)_

说明：设置文本文字的透明度，取值范围为0到1。

##### text-halo-fill `color`

默认值： #FFFFFF _(白色)_

说明：设置文本文字边缘的光晕颜色。

##### text-halo-radius `float`

默认值： 0 _(无光晕)_

说明：设置文本文字边缘的光晕大小，以像素为单位。

##### text-halo-rasterizer `keyword`

取值范围：`full` `fast`

默认值： full

说明：设置用于渲染文字光晕的方法，速度优先还是质量优先。

##### text-dx `float`

默认值： 0

说明：设置文本文字的水平偏移量，以像素为单位。正值表示向右偏移。

##### text-dy `float`

默认值： 0

说明：设置文本文字的垂直偏移量，以像素为单位。正值表示向下偏移。

##### text-vertical-alignment `keyword`

取值范围：`top` `middle` `bottom` `auto`

默认值： auto _(自动，但受到dy值的影响。当dy\>0时，取bottom；而当dy\<0时，取top)_

说明：设置文本符号相对于点要素座标的位置。

##### text-avoid-edges `boolean`

默认值： false

说明：设置是否避免将文本标注置于绘制区域（通常为瓦片）的边缘处。

##### text-min-distance `float`

默认值： undefined

说明：设置文本符号之间的最小间距。

##### text-min-padding `float`

默认值： undefined

说明：设置文本符号在元瓦片中的最小边距。

##### text-min-path-length `float`

默认值： 0 _(无论路线长度是多少，都要绘制文本符号)_

说明：如果设置了该值，那么只有在当路线长度大于该值的时候才绘制文本符号。

##### text-allow-overlap `boolean`

默认值： false _(不允许文本符号相互压盖)_

说明：设置是否显式相互压盖的文本符号。

##### text-orientation `expression`

默认值： undefined

说明：设置文本旋转。

##### text-placement `keyword`

取值范围：`point` `line` `vertex` `interior`

默认值： point

说明：设置文本符号在对应几何要素上的放置方式。

##### text-placement-type `keyword`

取值范围：`dummy` `simple`

默认值： dummy

说明：设置文本符号之间相互避让的算法。simple表示使用由text-placements属性指定的基本算法。而dummy则表示不使用该特性。

##### text-placements `string`

默认值： 

说明：如果placement-type属性被设置为simple，那么就会依据该属性的值（即形如“POSITIONS, [SIZES]”的字符串）执行文本符号相互避让算法。例如：`text-placements: "E,NE,SE,W,NW,SW";`

##### text-transform `keyword`

取值范围：`none` `uppercase` `lowercase` `capitalize`

默认值： none

说明：设置是否对文本字符进行大小写转换。

##### text-horizontal-alignment `keyword`

取值范围：`left` `middle` `right` `auto`

默认值： auto

说明：设置文本文字的水平对齐方式。

##### text-align `keyword`

取值范围：`left` `right` `center` `auto`

默认值： auto _(默认的自动方式是居中对齐，但如果已经设置了`placement-type`属性，那么就会依据`text-placements`属性的值来对文字进行靠左或靠右对齐)_

说明：设置文本文字的对齐方式。

##### text-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。

##### text-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

## 2.7 盾标符号（shield）的属性

##### shield-name `expression`

默认值： undefined

说明：设置盾标上显示的标注文字。可以通过用中括号括起来的字段名来指定要使用的数据字段，例如[column\_name]。

##### shield-file `uri`

默认值： none

说明：设置显示在盾标文本后面的背景图片。



##### shield-face-name `string`

默认值： 

说明：设置盾标上标注文字的字体与样式。



##### shield-unlock-image `boolean`

默认值： false _(盾标文字将被置于盾标背景图片的中心位置)_

说明：设置盾标文字与背景图片之间的位置关系。如果不想把盾标文本绘制在背景图的中心，那么就应该将该属性值设置为true。



##### shield-size `float`

默认值： undefined

说明：设置盾标文字的大小，以像素为单位。



##### shield-fill `color`

默认值： undefined

说明：设置盾标文字的颜色。



##### shield-placement `keyword`

取值范围：`point` `line` `vertex` `interior`

默认值： point

说明：设置盾标的放置方式。point方式是将盾标置于点要素的位置，line方式是将盾标在线要素上沿线绘制多次，vertex方式是将盾标置于多边形的顶点位置，而interior方式则是将盾标置于面要素的内部。



##### shield-avoid-edges `boolean`

默认值： false

说明：设置是否避免在地图或瓦片的边缘处绘制盾标。



##### shield-allow-overlap `boolean`

默认值： false _(不允许盾标与其它现有地图要素重叠)_

说明：该属性用于设置在盾标与地图上其它符号出现压盖时，是否显示盾标。



##### shield-min-distance `float`

默认值： 0

说明：设置相邻两个盾标符号（可以是相同的盾标，也可以是不同的）之间的最小距离。



##### shield-spacing `float`

默认值： 0

说明：设置在同一线要素上多次绘制的盾标之间的间隔。



##### shield-min-padding `float`

默认值： 0

说明：设置盾标在瓦片上绘制时的最小边距。



##### shield-wrap-width `unsigned`

默认值： 0

说明：设置盾标文本多长的时候需要折行。



##### shield-wrap-before `boolean`

默认值： false

说明：控制盾标文本的折行动作。如果该值为false，那么每一行文本都会比wrap-width属性设定的值略长。



##### shield-wrap-character `string`

默认值：  

说明：设置盾标文本的折行字符。



##### shield-halo-fill `color`

默认值： #FFFFFF _(白色)_

说明：设置盾标文本的光晕颜色。



##### shield-halo-radius `float`

默认值： 0 _(盾标文本无光晕效果)_

说明：设置盾标文本光晕的大小，单位为像素。



##### shield-character-spacing `unsigned`

默认值： 0

说明：设置盾标文字的字间距。该属性目前仅适用于点要素上的盾标。



##### shield-line-spacing `unsigned`

默认值： undefined

说明：设置盾标文本中的行距，以像素为单位。



##### shield-text-dx `float`

默认值： 0

说明：设置盾标文本的水平偏移量，以像素为单位。正值表示向右偏移。



##### shield-text-dy `float`

默认值： 0

说明：设置盾标文本的垂直偏移量，以像素为单位。正值表示向下偏移。



##### shield-dx `float`

默认值： 0

说明：设置盾标本身的水平偏移量，以像素为单位。正值表示向右偏移。



##### shield-dy `float`

默认值： 0

说明：设置盾标本身的垂直偏移量，以像素为单位。正值表示向下偏移。



##### shield-opacity `float`

默认值： 1

说明：设置盾标背景图片的透明度。



##### shield-text-opacity `float`

默认值： 1

说明：设置盾标文本的透明度。



##### shield-horizontal-alignment `keyword`

取值范围：`left` `middle` `right` `auto`

默认值： auto

说明：设置盾标相对于其中心点的水平对齐方式。



##### shield-vertical-alignment `keyword`

取值范围：`top` `middle` `bottom` `auto`

默认值： middle

说明：设置盾标相对于其中心点的垂直对齐方式。



##### shield-placement-type `keyword`

取值范围：`dummy` `simple`

默认值： dummy

说明：设置盾标之间相互避让的算法。simple表示使用由shield-placements属性指定的基本算法。而dummy则表示不使用该特性。



##### shield-placements `string`

默认值： 

说明：如果shield-placement-type属性被设置为simple，那么就会依据该属性的值（即形如“POSITIONS, [SIZES]”的字符串）执行盾标相互避让算法。例如：`shield-placements: "E,NE,SE,W,NW,SW";`



##### shield-text-transform `keyword`

取值范围：`none` `uppercase` `lowercase` `capitalize`

默认值： none

说明：设置盾标文字的大小写方式。



##### shield-justify-alignment `keyword`

取值范围：`left` `center` `right` `auto`

默认值： auto

说明：设置盾标文本的对齐方式。



##### shield-transform `functions`

默认值： _(无变换)_

说明：设置SVG的变换函数。



##### shield-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。



##### shield-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。


## 2.8 线模式（line-pattern）的属性

##### line-pattern-file `uri`

默认值： none

说明：设置沿线重复绘制的图像文件。



##### line-pattern-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。



##### line-pattern-simplify `float`

默认值： 0 _(不对几何要素进行简化)_

说明：如果要对几何要素按照地图综合的方法进行简化，那么通过该属性来设定阈值。



##### line-pattern-simplify-algorithm `keyword`

取值范围：`radial-distance` `zhao-saalfeld` `visvalingam-whyatt`

默认值： radial-distance _(不使用radial-distance算法进行简化)_

说明：设置对线要素进行综合的简化算法。



##### line-pattern-smooth `float`

取值范围：0到1

默认值： 0 _(不进行平滑处理)_

说明：对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。



##### line-pattern-offset `float`

默认值： 0 _(无偏移)_

说明：将线要素相对于其原有位置向左（沿着线的走向）或向右偏移一定量的像素绘制。正值表示左偏，负值表示右偏。



##### line-pattern-geometry-transform `functions`

默认值： none _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。



##### line-pattern-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。


## 2.9 面模式（polygon-pattern）的属性

##### polygon-pattern-file `uri`

默认值： none

说明：设置用于平铺填充面要素的图像文件。



##### polygon-pattern-alignment `keyword`

取值范围：`local` `global`

默认值： local

说明：设置填充时的对齐方式，local指在当前图层中对齐，global指在整个地图中对齐。



##### polygon-pattern-gamma `float`

取值范围：0到1

默认值： 1 _(完全抗锯齿)_

说明：设置面要素边缘的抗锯齿级别，影响绘制效果和速度。



##### polygon-pattern-opacity `float`

默认值： 1 _(保持填充图片的透明度不变)_

说明：设置填充图片的透明度。



##### polygon-pattern-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。



##### polygon-pattern-simplify `float`

默认值： 0 _(不对面要素的边线进行简化)_

说明：如果要对面要素的边线按照地图综合的方法进行简化，那么通过该属性来设定阈值。



##### polygon-pattern-simplify-algorithm `keyword`

取值范围：`radial-distance` `zhao-saalfeld` `visvalingam-whyatt`

默认值： radial-distance _(不使用radial-distance算法进行简化)_

说明：设置对面要素的边线进行综合的简化算法。



##### polygon-pattern-smooth `float`

取值范围：0到1

默认值： 0 _(不对拐点进行平滑)_

说明：对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。



##### polygon-pattern-geometry-transform `functions`

默认值： none _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。



##### polygon-pattern-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。


## 2.10 栅格符号（raster）的属性

##### raster-opacity `float`

默认值： 1 _(不透明)_

说明：设置栅格符号的透明度。



##### raster-filter-factor `float`

默认值： -1 _(允许数据源自行选用缩小图像尺寸的方法)_

说明：用于栅格或GDAL数据源，对图像尺寸进行预先缩小。将该值调高可以得到更好的缩略图效果，但相应的处理时间也会变长。



##### raster-scaling `keyword`

取值范围：`near` `fast` `bilinear` `bilinear8` `bicubic` `spline16` `spline36` `hanning` `hamming` `hermite` `kaiser` `quadric` `catrom` `gaussian` `bessel` `mitchell` `sinc` `lanczos` `blackman`

默认值： near

说明：设置对栅格数据进行重采样的算法。Bilinear可以在速度和质量方面得到不错的平衡，而lanczos则能够得到最高的绘制质量。



##### raster-mesh-size `unsigned`

默认值： 16 _(取原始图像分辨率的1/16作为栅格的重投影网格大小)_

说明：在对原始图像进行重投影时，是先将图像切分成若干网格，对网格中的小图像片分别重投影。如果设定该值使得网格的尺寸变大（即把该属性的值调小），那么重投影的速度会加快，但可能会导致图像变形。



##### raster-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。


## 2.11 注记符号（markers）的属性

##### marker-file `uri`

默认值：  _(一个椭圆或正圆形符号)_

说明：设置绘制注记符号所使用的SVG文件。如果没有指定具体的文件，则默认使用一个椭圆形符号对每个位置的注记进行渲染。



##### marker-opacity `float`

默认值： 1 _(边缘和填充均不透明)_

说明：设置注记符号整体的透明度。如果设置了该属性，则会覆盖在marker-fill-opacity和marker-line-opacity属性中设置的透明度。



##### marker-fill-opacity `float`

默认值： 1 _(注记符号填充部分为不透明)_

说明：设置注记符号填充部分的透明度。



##### marker-line-color `color`

默认值： black

说明：设置注记符号边线的颜色。



##### marker-line-width `float`

默认值： undefined

说明：设置注记符号边线的宽度，以像素为单位。但如果该值设置过大会导致注记本身被过粗的边线覆盖。



##### marker-line-opacity `float`

默认值： 1 _(注记符号边线完全不透明)_

说明：设置注记符号边线的透明度。



##### marker-placement `keyword`

取值范围：`point` `line` `interior`

默认值： point _(注记符号被置于几何要素的重心（形心）位置)_

说明：设置注记在几何要素上的放置方式，可以位于点要素上，或者在面要素的中心位置，还可以沿着线要素反复出现（通过设置`marker-placement:line`实现）。如果取值interior，那么可以确保注记符号被绘制在多边形的内部。



##### marker-multi-policy `keyword`

取值范围：`each` `whole` `largest`

默认值： each _(如果一个要素包含了多个几何形状，而且放置方式是point或interior，那么注记符号就会在每个几何形状处都会被绘制一次)_

说明：该属性是为包含多个几何形状的地理（multi-geometries）要素准备的，对沿线放置的注记符号不起作用。其默认值为each，也就是每个几何形状上都会被绘制一个注记；whole表示注记将被绘制在所有几何形状组合后的重心位置；而largest表示注记将被绘制在最大（依据最小包围框的面积）的那个几何形状上（这也同样是文本标注在多几何要素上绘制的默认方法）。



##### marker-type `keyword`

取值范围：`arrow` `ellipse`

默认值： ellipse

说明：设置默认的注记符号类型。如果没有指定用于渲染注记的SVG文件，那么内置的渲染引擎可以提供两种选择：箭头或椭圆。



##### marker-width `expression`

默认值： 10

说明：设置注记符号的宽度。这个属性只适用于两种内置的默认注记样式。



##### marker-height `expression`

默认值： 10

说明：设置注记符号的高度。这个属性只适用于两种内置的默认注记样式。



##### marker-fill `color`

默认值： blue

说明：设置注记的填充色。



##### marker-allow-overlap `boolean`

默认值： false _(不允许注记符号相互压盖（被压盖的注记将不被显式）)_

说明：设置被压盖的注记符号是否被显式在地图上。



##### marker-ignore-placement `boolean`

默认值： false _(不在冲突检测器的缓存中存储几何形状的外包框)_

说明：设置是否允许在与当前要素重叠的位置放置其它要素。



##### marker-spacing `float`

默认值： 100

说明：设置重复绘制的注记之间的间距，单位为像素。如果设定的间距小于注记符号本身的尺寸，或者大于线要素的长度，那么注记就绘制不出来。



##### marker-max-error `float`

默认值： 0.2

说明：设置实际的注记位置与marker-spacing属性值之间的最大误差。如果将该属性值调高，那么渲染引擎就会尝试处理与其它注记符号之间的位置冲突。



##### marker-transform `functions`

默认值： _(无变换)_

说明：设置SVG图形的变换方法。



##### marker-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。



##### marker-smooth `float`

取值范围：0到1

默认值： 0 _(不对拐点进行平滑)_

说明：对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。



##### marker-geometry-transform `functions`

默认值： none _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。



##### marker-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。


## 2.12 建筑物符号（building）的属性

##### building-fill `color`

默认值： #FFFFFF _(白色)_

说明：设置建筑物的外墙填充色



##### building-fill-opacity `float`

默认值： 1

说明：设置建筑物整体的透明度，包括建筑物所有的面。



##### building-height `expression`

默认值： 0

说明：设置建筑物的高度，以像素为单位。


## 2.13 调试模式下的属性

##### debug-mode `string`

默认值： collision

说明：设置调试模式渲染


## 2.14 关于取值类型的说明

这里列出了CartoCSS中所有属性的取值类型及其说明。

### 颜色型（Color）

CartoCSS可以使用一系列不同的方法表示颜色：HTML风格的16进制值，RGB值，RGBA值，HSL值或HSLA值都可以，还可以使用HTML预定义颜色名，像`yellow`、`blue`等。

	
	#line {
	 line-color: #ff0;
	 line-color: #ffff00;
	 line-color: rgb(255, 255, 0);
	 line-color: rgba(255, 255, 0, 1);
	 line-color: hsl(100, 50%, 50%);
	 line-color: hsla(100, 50%, 50%, 1);
	 line-color: yellow;
	}
	

这里特别需要强调的是对HSL值的支持，这其实是比RGB值更易用的颜色表达方式（参见[这里](http://mothereffinghsl.com/)）。CartoCSS中还支持几种颜色函数，这是从LESS中借用的概念（参见[这里](http://lesscss.org/#-color-functions)），例子如下：

	
	// 把颜色调亮或调暗
	lighten(#ace, 10%);
	darken(#ace, 10%);
	
	// 饱和度调高或调低
	saturate(#550000, 10%);
	desaturate(#00ff00, 10%);
	
	// 提高或降低颜色的透明度
	fadein(#fafafa, 10%);
	fadeout(#fefefe, 14%);
	
	// 按照一定角度旋转色盘
	spin(#ff00ff, 10);
	
	// 将两种颜色混合
	mix(#fff, #000, 50%);
	

以上这些函数的参数可以是颜色值，也可以是颜色名，还可以是其它颜色函数。

### 浮点型（Float）

浮点数是数值类型的时髦说法。在CartoCSS中，这指的就是一个数值，没有单位，但其实所有的单位都是像素。

	
	#line {
	 line-width: 2;
	}
	

还可以对数值类型做简单运算：

	
	#line {
	 line-width: 4 / 2; // 除
	 line-width: 4 + 2; // 加
	 line-width: 4 - 2; // 减
	 line-width: 4 * 2; // 乘
	 line-width: 4 % 2; // 取余
	}
	

### 统一资源描述符型（URI）

当一个属性的值类型是URI时，用户可以像在HTML中使用`url('place.png')`一样的表示方法。URL地址上的引号不是必需的，但最好加上。URI可以指向本地文件系统，也可以是互联网上资源的链接地址。

	
	#markers {
	 marker-file: url('marker.png');
	}
	

### 字符串型（String）

字符串也就是文本类型。在CartoCSS中，字符串应该有引号包围。字符串可以是任意文本，但在`text-name`和`shield-name`属性中，可以使用中括号包围的数据字段名来表示。例如：

	
	#labels {
	 text-name: "[MY_FIELD]";
	}
	

### 布尔型（Boolean）

布尔类型即是或否，取值为`true`或`false`。

	
	#markers {
	 marker-allow-overlap:true;
	}
	

### 表达式型（Expressions）

表达式是一种语句，它可以将数据字段、数值以及其它类型灵活的组合起来。前面提到的`"[FIELD]"`形式包含了表达式。实际的表达式可以不用加引号就执行加、减、乘、除、连接等CartoCSS语法支持的操作。

	
	#buildings {
	 building-height: [HEIGHT_FIELD] * 10;
	}
	

### 数列型（Numbers）

数列型是逗号分隔的一组有序数值。数列类型在用于配置虚线样式时，其中的数字交替表示的是实线段长度、间隔长度和实线段长度。

	
	#disputedboundary {
	 line-dasharray: 1, 4, 2;
	}
	

### 百分数型（Percentages）

在CartoCSS中，百分号`%`表示`值/100`。它可以用于表示比例的属性，例如透明度。

_注意，百分数不能用于定义宽度、高度等属性。这一点与CSS不同，因为在CartoCSS中没有CSS中层次化的页面要素和页宽。它们在这里只是除以100以后的值。_

	
	#world {
	 // 这种表达方式与...
	 polygon-opacity: 50%;
	
	 // ...这种方式效果一样
	 polygon-opacity: 0.5;
	}
	

### 函数型（Functions）

这种类型可以包含一组逗号分隔的函数。例如，各种变换都是用`functions`作为值类型，而且这些函数还可以串接起来。

	
	#point {
	 point-transform: scale(2, 2);
	}
	

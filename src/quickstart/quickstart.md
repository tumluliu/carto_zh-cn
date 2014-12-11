## 快速入门

译注：根据MapBox Studio文档中的[QuickStart](https://www.mapbox.com/mapbox-studio/style-quickstart/)部分撰写

Mapbox Studio uses a language called CartoCSS to determine the look of a map. Colors, sizes, and shapes can all be manipulated by applying their specific CartoCSS parameters in the stylesheet panel to the right of the map. Read the CartoCSS manual for a more detailed introduction to the language.

CartoCSS可以对地图中各种要素样式的细节进行控制。包括颜色、大小、形状等，都可以通过设置CartoCSS的各种属性参数来实现制图样式的配制。

In this tutorial we’ll create a custom style by writing CartoCSS for buildings, roads, and parks.

为了让读者快速的了解用CartoCSS能配出什么样的地图，如何配出这样的地图，本章以房屋、公园和道路的制图样式配置为例，简单展示一下CartoCSS的制图能力。

译注：以下例子为在Mapbox Studio中进行制图的实例，但并不局限于Mapbox Studio。而且这些例子需要有对应的数据准备。我们先假定所需要的数据已经准备好。

### 配置房屋样式（Styling buildings）

Add the following CartoCSS to your custom stylesheet and then click Save.

为了对一个表示建筑物轮廓的面要素矢量数据集进行样式配置，可使用以下CartoCSS脚本：

	
	#building[zoom>=14] {
	 polygon-fill:#eee;
	 line-width:0.5;
	 line-color:#ddd;
	}
	

制图渲染效果如下图：

![](https://cloud.githubusercontent.com/assets/83384/3870305/ba0d0a6a-20c7-11e4-9454-a751319ca7e2.png)

_图片来源：www.mapbox.com_

- `#building` selects the building layer as the one that will be styled.
- `[zoom>=14]` restricts the styles to zoom level 14 or greater.
- `polygon-fill: #eee` fills the building polygons with a light grey color.

- `#building`标识了需要进行样式配置的图层；
- `[zoom>=14]`指定了该样式只有在地图缩放级别大于等于14级的时候才起作用；
- `polygon-fill: #eee`设定了房屋多边形的填充色为浅灰。

To add depth to our buildings at higher zoom levels let’s add another set of rules that use the building symbolizer to render polygons as block-like shapes. Add the following CartoCSS to your custom stylesheet and then click Save.

我们还可以让房屋在更高的缩放级别上展示出具有一定高度的效果，类似于一个个积木块。为此，还需要添加这么一段代码：

	
	#building[zoom>=16] {
	 building-fill:#eee;
	 building-fill-opacity:0.9;
	 building-height:4;
	}
	

修改后的制图渲染效果如下图：

![](https://cloud.githubusercontent.com/assets/83384/3870329/bceff796-20c8-11e4-8ff2-23bf7b374bff.png)

_图片来源：www.mapbox.com_

### 配置公园样式（Styling parks）

Add the following CartoCSS to your custom stylesheet and then click Save.

这次我们试一下对公园这种类别的地块配置制图样式。

	
	#landuse[class='park'] {
	 polygon-fill:#dec;
	}
	
	#poi_label[maki='park'][scalerank<=3][zoom>=15] {
	 text-name:@name;
	 text-face-name:@sans;
	 text-size:10;
	 text-wrap-width: 60;
	 text-fill:#686;
	 text-halo-fill:#fff;
	 text-halo-radius:1;
	 text-halo-rasterizer:fast;
	}
	

通过使用以上CartoCSS脚本可以得到如下渲染效果：

![](https://cloud.githubusercontent.com/assets/83384/3870363/c7b51674-20c9-11e4-8393-9da2f75b5d67.png)

_图片来源：www.mapbox.com_

下面逐一介绍其中用到的CartoCSS关键要素：

- `#landuse` selects features from the landuse layer.
- `[class='park']` restricts the landuse layer to features where the class attribute is park.
- `#poi_label` selects the `poi_label` layer for labelling parks.
- `[maki='park'][scalerank<=3][zoom>=15]` restricts the `poi_label` layer to prominent park labels and restricts their visibility to zoom level 15 or greater. 
- `text-name: @name` sets the field that label contents will use for their text. It references the existing `@name` variable defined in the `style` tab. 
- `text-face-name: @sans` sets the font to use for displaying labels. It references the existing `@sans` variable defined in the style tab. 
- `text-wrap-width: 60` sets a maximum width for a single line of text. 
- `text-halo-rasterizer: fast` uses an alternative optimized algorithm for drawing halos around text that improves rendering speed.

- `#landuse`标识了公园地块数据所在的图层，即`landuse`层；
- `[class='park']`修饰符利用条件过滤器限定了该样式的作用范围，即class属性值为park的那些面要素；
- `#poi_label`标识了用于对公园进行文字标注的图层，即`poi_label`层；
- `[maki='park'][scalerank<=3][zoom>=15]`修饰符利用了一组条件过滤器限定了该样式在`poi_label`层中的作用范围，以及满足这些条件的标注只有在缩放级别大于15级的时候才可见；
- `text-name: @name`用于设置用于标注公园的数据字段，以一个变量`@name`的形式给出，`@name`可以在之前先定义好，例如`@name: '[name_en]';`
- `text-face-name: @sans`用于设置文本标注的字体。与`text-name`一样，这里也使用了变量，即`@sans`；
- `text-wrap-width: 60`设置了文本标注中每一行的最大长度；
- `text-halo-rasterizer: fast`指定使用一种经过优化的快速绘制方法来渲染标注文字的光晕。

### 标注道路（Labelling roads）

Add the following CartoCSS to your custom stylesheet and then click Save.

前面两个例子中都是对矢量数据中的面要素类型进行样式配置。在这个例子中，我们以道路为例，看看如何配置线要素标注的样式。

	
	#road_label[zoom>=13] {
	 text-name:@name;
	 text-face-name:@sans;
	 text-size:10;
	 text-placement:line;
	 text-avoid-edges:true;
	 text-fill:#68a;
	 text-halo-fill:#fff;
	 text-halo-radius:1;
	 text-halo-rasterizer:fast;
	}
	

以上CartoCSS脚本将产生如下渲染效果：

![](https://cloud.githubusercontent.com/assets/83384/3870380/23717e70-20cb-11e4-99f5-68a80914a0ce.png)

_图片来源：www.mapbox.com_

其中涉及到的主要属性和参数的含义如下：

- `#road_label` selects features from the `road_label` layer.
- `[zoom>=13]` restricts the `road_label` layer to zoom level 13 or greater.
- `text-placement: line` sets labels to follow the orientation of lines rather than horizontally.
- `text-avoid-edges: true` forces labels to be placed away from tile edges to avoid being clipped.

- `#road_label`标识了道路标注对应的图层为`road_label`层；
- `[zoom>=13]`限定了只有在缩放级别大于13级的时候才显示道路标注；
- `text-placement: line`设置标注的放置方式为沿着线的走向显示文字，而不是规则的沿水平方向显示文字；
- `text-avoid-edges: true`强制所有标注要避开绘制区域（例如瓦片）的边缘，防止出现被切断的情况。
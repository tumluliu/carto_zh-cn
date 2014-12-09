## 快速入门

TODO: 根据MapBox Studio文档中的[QuickStart](https://www.mapbox.com/mapbox-studio/style-quickstart/)部分撰写

Mapbox Studio uses a language called CartoCSS to determine the look of a map. Colors, sizes, and shapes can all be manipulated by applying their specific CartoCSS parameters in the stylesheet panel to the right of the map. Read the CartoCSS manual for a more detailed introduction to the language.

In this tutorial we’ll create a custom style by writing CartoCSS for buildings, roads, and parks.

### Styling buildings
Add the following CartoCSS to your custom stylesheet and then click Save.

	
	#building[zoom>=14] {
	 polygon-fill:#eee;
	 line-width:0.5;
	 line-color:#ddd;
	}
	

![](https://cloud.githubusercontent.com/assets/83384/3870305/ba0d0a6a-20c7-11e4-9454-a751319ca7e2.png)
_图片来源：www.mapbox.com_

- `#building` selects the building layer as the one that will be styled.
- `[zoom>=14]` restricts the styles to zoom level 14 or greater.
- `polygon-fill: #eee` fills the building polygons with a light grey color.

To add depth to our buildings at higher zoom levels let’s add another set of rules that use the building symbolizer to render polygons as block-like shapes. Add the following CartoCSS to your custom stylesheet and then click Save.

	
	#building[zoom>=16] {
	 building-fill:#eee;
	 building-fill-opacity:0.9;
	 building-height:4;
	}

![](https://cloud.githubusercontent.com/assets/83384/3870329/bceff796-20c8-11e4-8ff2-23bf7b374bff.png)
_图片来源：www.mapbox.com_

### Styling parks

Add the following CartoCSS to your custom stylesheet and then click Save.

	
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
	

![](https://cloud.githubusercontent.com/assets/83384/3870363/c7b51674-20c9-11e4-8393-9da2f75b5d67.png)
_图片来源：www.mapbox.com_

- `#landuse` selects features from the landuse layer.
- `[class='park']` restricts the landuse layer to features where the class attribute is park.
- `#poi_label` selects the `poi_label` layer for labelling parks.
- `[maki='park'][scalerank<=3][zoom>=15]` restricts the `poi_label` layer to prominent park labels and restricts their visibility to zoom level 15 or greater.
- `text-name: @name` sets the field that label contents will use for their text. It references the existing `@name` variable defined in the `style` tab.
- `text-face-name: @sans` sets the font to use for displaying labels. It references the existing `@sans` variable defined in the style tab.
- `text-wrap-width: 60` sets a maximum width for a single line of text.
- `text-halo-rasterizer: fast` uses an alternative optimized algorithm for drawing halos around text that improves rendering speed.

### Labelling roads

Add the following CartoCSS to your custom stylesheet and then click Save.

	
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
	

![](https://cloud.githubusercontent.com/assets/83384/3870380/23717e70-20cb-11e4-99f5-68a80914a0ce.png)
_图片来源：www.mapbox.com_

- `#road_label` selects features from the road\_label layer.
 - `[zoom>=13]` restricts the road\_label layer to zoom level 13 or greater.
- `text-placement: line` sets labels to follow the orientation of lines rather than horizontally.
- `text-avoid-edges: true` forces labels to be placed away from tile edges to avoid being clipped.
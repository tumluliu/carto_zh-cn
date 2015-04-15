
### 4.3 高级标注方法

#### 试探标注位置

在默认情况下，如果对一个点符号进行文本标注，那么标注将被绘制在原始几何点的位置处。这在大部分情况下没什么问题，但有的时候我们会希望从地图总体设计的角度去考虑标注的摆放问题，比如在标注比较密集的地方让它们向周围散一散，让POI的标注尽量避开主干道路网，等等。在这种时候，我们希望允许标注可以不必精确绘制在原始点的位置，而是可以稍有偏移。这种需求在CartoCSS中是可以实现的。在CartoCSS中，标注的位置可以通过`text-placement-type`属性来配置，它目前能够支持两种标注方案：一是`none`（该属性的默认值），效果是将标注标在原始点处；二是`simple`，别看它名字叫“simple”，实际上却是个高级标注方法。

`simple`方法允许设计师在原始点的周围为标注另外指定几个候选的摆放位置和字号大小。如果在默认位置渲染标注时会与现有的其它标注冲突，那么就从这些指定的其它候选位置中逐个尝试绘制，直到可以把标注绘制出来为止。而如果在所有候选位置都无法绘制，那就不画它了。

下面是一个较为完整的例子：

	
	#labels {
	  text-name: "[name]";
	  text-face-name: "OpenSans Regular";
	  text-placement-type: simple;
	  text-placements: "N,S,E,W,NE,SE,NW,SW,16,14,12";
	  text-dy: 3;
	  text-dx: 3;
	}
	

这段代码的意思是依次在原始点的上方（北方）、下方、右侧、左侧等八个位置尝试以16号字绘制标注。如果这些位置都不行，那么就换成14号字再试一遍，如果还不行，就再换成12号字试。如果上面这些尝试都画不出来，那么就跳过不画这个标注了。

`text-dx`和`text-dy`属性指定了标注位置相对于原始点位置的偏移量（以像素为单位）。

#### 改进标注的排布：随机法

尽管前面这个在多个位置尝试绘制标注的想法不错，但它的问题在于每次的试探位置序列都是完全一样的，例如在上面的例子中就是`N,S,E,W,NE,SE,NW,SW `这个顺序。从地图整体美感的角度来看，这通常不是最好的方案，即使标注都能找到自己在地图上的“容身之所”。那么为了能从地图总体设计的角度来将所有的标注合理排布，我们需要考虑另外的试探绘制方法，比如不总是按照同样的位置序列来尝试画标注。

Something as simple as randomly assigning a direction bias can help even out the look of the labels. For example, you could create a PostGIS query that creates a column called dir which is randomly assigned a value of either `0` or `1`.

有个很简单的改进方法，就是只要将试探绘制的位置序列由固定的改为随机的便可以让标注的分布变得美观许多。具体而言，你可以利用一条PostGIS的SQL语句为原始点数据增加一个名为`dir`的新列（译注：注意这不是说要去修改原始数据，而是通过SQL语句生成一个临时的属性列），它的值是随机生成的`0`或`1`。

	
	(select *, floor(random()*2) as dir from city_points) as data
	

然后就可以基于`dir`的值，让每个点的标注试探位置序列分别在`dir`等于`0`时为`E,NE,SE,W,NW,SW`；等于`1`时为`W,NW,SW,E,NE,SE`。

	
	#labels {
	  text-name: "[name]";
	  text-face-name: "OpenSans Regular";
	  text-placement-type: simple;
	  text-placements: "E,NE,SE,W,NW,SW";
	  [dir=1] { text-placements: "W,NW,SW,E,NE,SE"; }
	}
	

#### 改进标注的排布：邻居避让法

前面我们稍稍利用了一下PostGIS和SQL，就得到了一种让标注排布更加合理的方案。其实那只是PostGIS和SQL强大能力的冰山一角。现在就让我们再深入一点，利用它们实现一种比随机法更加美观合理的标注排布方案——邻居避让法。在这种方法中，先找到距离当前标注点最近的邻居对象，看看它的标注是向哪偏移。然后在绘制标注时尽量避开这个最近邻居的标注。例如，在为城市标注名称时，可以让每个城市的名称都稍作偏移以避开离它最近的另一个城市，而在标注地区名称时则尽量让其避开该地区中最大城市的名字，以防止出现标注冲突导致的无法绘制。这些方法和实践未必是最佳方案，但对于大多数情况来说可以让你的标注排布更加合理。

这里我们讨论一个应用邻居避让法的典型场景。对于那些正好位于[城市街区](https://zh.wikipedia.org/wiki/%E8%A1%97%E5%8D%80)边缘附近的兴趣点，它们的名称完全可以尽量标注在街区覆盖的区域中，而避开其临近的城区街道。将这些标注置于街区区域还可以保持街道名称和通行方向等道路标注信息清晰可见。那么如何达到这种效果呢？思路并不复杂。对于每个点标注，找到距离它最近的城市街道以及这条街道相对于它的方位。在搜索最近邻街道的时候可以忽略一些低等级道路（例如OpenStreetMap数据集中的service streets、tracks、footways和cycleways等），但也可以根据实际情况对避让策略进行调整。在大部分时候，将标注置于一条小巷或公园小路上是可以接受的，但城市主干道不应该被其附近的点标注压盖。

那么又如何利用PostGIS和SQL来具体实现呢？在PostGIS中，有一系列空间操作函数，可以帮助我们实现上面的避让策略：

- [`ST_Distance`](http://www.postgis.org/docs/ST_Distance.html)函数可以帮助我们找到距离一个兴趣点最近的道路
- [`ST_ClosestPoint`](http://www.postgis.org/docs/ST_ClosestPoint.html)函数则可以找到在最近的道路上的最近的几何形点
- [`ST_Azimuth`](http://www.postgis.org/docs/ST_Azimuth.html)函数可以帮助我们计算从当前兴趣点到其最近形点的方位夹角

利用这些函数，我们可以写一个PostgreSQL函数。但需要特别说明的是，这个函数假设你已经通过[`osm2pgsql`](http://wiki.openstreetmap.org/wiki/Osm2pgsql)准备好了一个标准的OpenStreetMap数据库，其中的属性和值都是针对OpenStreetMap数据结构的。你当然可以对它进行修改以适应其它的数据库结构。

	
	create or replace function poi_ldir(geometry)
	    returns double precision as
	$$
	    select degrees(st_azimuth(st_closestpoint(way, $1),$1)) as angle
	    from planet_osm_line
	    where way && st_expand($1, 100)
	        and highway in ('motorway', 'trunk', 'primary', 'secondary', 'tertiary',
	            'unclassified', 'residential', 'living_street', 'pedestrian')
	    order by st_distance(way, $1) asc
	    limit 1
	$$
	language 'sql'
	stable;
	

函数最前面的两行定义了名称`poi_ldir`、参数和返回值。函数体从`$$`符号处开始。调用`poi_ldir`时需要传入一个几何点要素作为参数，而后将距离这个几何点最近的道路（道路类型由`where`子句确定）与该点之间夹角的角度算出并返回，结果的取值范围为`0`到`360`度。（注：`ST_Azimuth()`函数本来返回的是弧度，但为了在CartoCSS中方便使用，我们将其转成了角度值。）

如何让这个函数在数据库中可用呢？很简单，只要把它先存入一个文本文件（例如以`poi_ldir.sql`文件保存在桌面上）然后在终端中执行以下命令（译注：当然这里假定你使用的是Mac OS或Linux等\*nix类型的系统，如果是MS Windows则需要调整文件路径），那么这个函数就被创建在你的数据库`your_database_name `中了。当然，通过一些PostgreSQL的图形化客户端（例如pgAdmin、phppgsql等）可以利用菜单项中的创建函数功能来实现。

	
	psql -f ~/Desktop/poi_ldir.sql -d <your_database_name>
	

然后在制图过程中就可以使用这个函数了。以下这个查询语句将数据库中所有的设施和商店点要素取出来，结果中包括了每个要素的名称和名为`ldir`的属性列。`ldir`属性列就是通过`poi_ldir`函数计算出来的结果。

	
	( select way, name, poi_ldir(way) as ldir
	  from planet_osm_point
	  where amenity is not null or shop is not null
	) as pois
	

接下来，在CartoCSS样式表中怎么使用`ldir`属性列呢？在将`text-placement-type`属性设置为`simple`之后，内嵌一组基于`ldir`值的过滤器以调整`text-placements`属性的值。在下面的样式表例子中，每个标注只需在一个候选位置尝试绘制。

	
	#poi[zoom > 15] {
	  text-name: '[name]';
	  text-face-name: @sans_medium;
	  text-size: 12;
	  text-fill: #222;
	  text-wrap-width: 60;
	  text-wrap-before: true;
	  text-halo-radius: 2;
	  text-halo-fill: #fff;
	  text-min-distance: 2;
	  text-placement-type: simple;
	  text-dx: 5;
	  text-dy: 5;
	  text-placements: 'N';
	  [ldir >= 45][ldir < 135] { text-placements: 'E'; }
	  [ldir >= 135][ldir < 225] { text-placements: 'S'; }
	  [ldir >= 225][ldir < 315] { text-placements: 'W'; }
	}
	

将上面这段样式应用在一段完整的OpenStreetMap数据样式表中之后，可以看到其中绝大部分的点标注都避开了道路网。

![](https://www.mapbox.com/tilemill/assets/pages/labels-ldir.png)

#### 参考文献

1. Mapbox, [Advanced Label Placement](https://www.mapbox.com/tilemill/docs/guides/labels-advanced/)


### 高级标注方法

_译注：[原文地址](https://www.mapbox.com/tilemill/docs/guides/labels-advanced/)_

#### 尝试在其它位置标注（Trying multiple positions）

Recent versions of TileMill include two methods to choose for placing labels on points. The choice is made via the `text-placement-type` CartoCSS property. The default, original method is called `none`, and the newer, more advanced method is called `simple`.

CartoCSS通过`text-placement-type`属性支持两种在点符号上面放置标注的方法。其默认值是`none`，除此以外还有个高级方法——`simple`。

The `simple` method allows the designer to specify multiple potential positions on or around a central point, as well as multiple sizes of text to choose from. If the first attempt at placing a label is blocked by another label that has already been placed, it can look at this list to try the next position.

这个`simple`方法允许设计师在原始点的周围为标注另外指定几个候选位置和字号。如果在默认位置渲染某个标注时出现了被其它标注遮挡的情况，那么就会从这些指定的候选位置中逐个尝试绘制。

A full CartoCSS example of the syntax looks like this:

下面是一个较为完整的例子：

	
	#labels {
	  text-name: "[name]";
	  text-face-name: "OpenSans Regular";
	  text-placement-type: simple;
	  text-placements: "N,S,E,W,NE,SE,NW,SW,16,14,12";
	  text-dy: 3;
	  text-dx: 3;
	}
	

This will first attempt to place a label above a point, then below the point, then to the right, and so on with a text size of 16 until it finds a position that fits. If no labels fit at size 16, the positions will all be retried at a text size of 14, and then 12. If none of these fit the label will be skipped.

这段代码的作用是依次在原始点的上方（北方）、下方、右侧等进行尝试，如果位置合适，那么就用16号字绘制标注。如果这些位置都画不出来，那么就换成14号字再试一遍，如果还不行，就再换成12号字试。如果都不行，那么这个标注就会被跳过不画了。

The `text-dx` and `text-dy` properties specify how far away (in pixels) the label should be placed from the point.

`text-dx`和`text-dy`属性指定了标注位置相对于原始点位置的偏移量（以像素为单位）。

#### 改进标注位置的分布：随机法（Improved direction distribution: random approach）

It’s great to try different placements for a label, but the previous example will always try the same position (North) first. This may not always be the best choice, even if the label happens to fit there. And it may be better for the overall map design to distribute the different placement positions better, rather than letting a single position dominate.

尽管在多个不同的位置尝试放置标注的想法不错，但它的问题在于每次都是从同一个位置开始尝试。这往往不是最好的选择（译注：从地图设计和美观的角度），即使标注可以被绘制在那个位置。将标注位置更合理的分布，而不是总绘制在同一个位置也会带来更好的整体地图设计效果。

Something as simple as randomly assigning a direction bias can help even out the look of the labels. For example, you could create a PostGIS query that creates a column called dir which is randomly assigned a value of either `0` or `1`.

有个很简单的改进方法，就是只要将标注相对于原始点的偏移方向变成随机性的便可以让标注的分布变得美观许多。具体而言，你可以利用一条PostGIS的SQL语句为原始点数据增加一个名为`dir`的新列（译注：注意这不是说要去修改原始数据，而是通过SQL语句生成），它的值是随机生成的`0`或`1`。

	
	(select *, floor(random()*2) as dir from city_points) as data
	

You could then set up your CartoCSS to favor East placement for the `0`s and West placement for the `1`s.

然后就可以基于`dir`列的值，让每个点的标注在为`0`时向东、为`1`时向西偏移。

	
	#labels {
	  text-name: "[name]";
	  text-face-name: "OpenSans Regular";
	  text-placement-type: simple;
	  text-placements: "E,NE,SE,W,NW,SW";
	  [dir=1] { text-placements: "W,NW,SW,E,NE,SE"; }
	}
	

#### 改进标注位置的分布：邻居避让法（Improved direction distribution: avoiding nearby neighbors）

Using PostGIS its possible to come up with something smarter than random distribution to improve the look of simple label placement. One possibility is to calculate the direction of the nearest object of a certain type, and then try to avoid that. For example you could bias city lable placement away from the next nearest city, or county label placement away from the largest city in the county. These aren’t perfect solutions, but can be a quick way to make your labels more correct in more cases.

利用PostGIS可以得到比随机方法更加美观合理的标注排布。一种方法是先找到当前标注点的特定类型的最近邻对象，看看它的标注偏移方向，然后在绘制标注时尽量避开最近的邻居。例如在标注城市名称时，可以让每个城市的名字标注都稍作偏移以避开其最近的其它城市，而标注地区名称时则尽量让其避开该地区中最大城市的名字。这些方法和实践未必是最佳方案，但对于大多数情况来说可以让你的标注排布更加合理。

For labels on points-of-interest along a city block at high zoom level, the area most likely to have room for the label is away from the street. Placing labels here also keeps the street clear for its own labels and one-way arrows.

_译注：第一句没看明白意思。前半句里的along a city block是个什么东东？想象不出来。后半句的意思应该是：最有可能放得下标注的地方（正好）远离道路。_所以，将标注置于这里还可以保持道路自己的名字和通行方向等清晰可见。

So for each label we need to find the nearest city street and its direction relative to the point. Service streets, tracks, footways, and cycleways will be ignored for this logic, but you could adjust it to account for whatever you feel is appropriat. For a basic use case fine if our label sits on top of an alley or park path; the goal is to avoid the main city grid.

因而对于每个点标注，我们应该找到距离它最近的城市街道以及这条街道相对于原始点的方位。对于一些低等级道路（例如OpenStreetMap数据集中的service streets、tracks、footways和cycleways等），可以不用考虑，但也可以根据实际情况对标注的避让策略进行调整。通常情况下，点的标注被置于一条小巷或公园中的小路上是可以接受的，但城市的主干道不应该被点标注压盖。

Here are some of the spatial functions of PostGIS that will help determine this information:

以下是一些PostGIS中的空间操作函数，可以辅助实现上面的避让策略：

- [ST\_Distance](http://www.postgis.org/docs/ST_Distance.html) will help us find the closest street to a POI
- [ST\_ClosestPoint](http://www.postgis.org/docs/ST_ClosestPoint.html) will tell us the closest point along the closest street, and
- [ST\_Azimuth](http://www.postgis.org/docs/ST_Azimuth.html) will help us determine the angle between the POI and the closest point.

- [ST\_Distance](http://www.postgis.org/docs/ST_Distance.html)函数可以帮助我们找到距离一个兴趣点最近的道路
- [ST\_ClosestPoint](http://www.postgis.org/docs/ST_ClosestPoint.html)函数则可以找到在最近的道路上的最近的几何形点
- [ST\_Azimuth](http://www.postgis.org/docs/ST_Azimuth.html)函数可以帮助我们计算从当前兴趣点到其最近形点的方位夹角

We can put all these together as a user-defined PostreSQL function:

所有这些都可以写在一个PostgreSQL函数中：

	
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
	

This particular function assumes you are working with a standard OpenStreetMap rendering database generated by [osm2pgsql](http://wiki.openstreetmap.org/wiki/Osm2pgsql) (you can adjust it to be used with other schemas). The first two lines set up a function with a name, argument, and return value. `$$` starts the function. The result of the function, when given a point geometry as an argument, will be a number between 0 and 360 representing the angle between that point and the nearest street of any of the types defined in the `where` clause. (`ST_Azimuth()` returns a value in radians, but we convert that to degrees to make it easier to work with in CartoCSS.)

上面这个函数假设你已经通过[osm2pgsql](http://wiki.openstreetmap.org/wiki/Osm2pgsql)准备好了一个标准的OpenStreetMap数据库（你可以对它进行修改以适应其它的数据库结构）。最前面两行定义了函数的名称`poi_ldir`、参数和返回值。函数体从`$$`符号处开始。调用`poi_ldir`时需要传入一个几何点要素作为参数，而后将距离这个几何点最近的道路（道路类型由`where`子句确定）与该点之间的夹角以角度计算并返回，取值范围为0到360度。（`ST_Azimuth()`函数本来返回的是弧度，但为了在CartoCSS中方便使用，我们将其转成了角度值。）

To use the above function to your database, copy its contents to a file (for example, `poi_ldir.sql` on your Desktop). Then run a command from the terminal to load it into your database:

要使用上面这个函数，只需要把它存入一个文本文件（例如以`poi_ldir.sql`文件保存在桌面上）并在终端中执行以下命令即可：

	
	psql -f ~/Desktop/poi_ldir.sql -d <your_database_name>
	

You can then use the function in your TileMill select statements. This selection will retrieve all amenity and shop points from the database, their names, and column named `ldir` that is the result of the `poi_ldir` function on the geometry for each point.

然后在制图过程中就可以使用这个函数了。以下这个查询语句将数据库中所有的设施和商店点要素取出来，结果中包括了每个要素的名称和名为`ldir`的属性列。`ldir`属性列就是通过`poi_ldir`函数计算出来的结果。

	
	( select way, name, poi_ldir(way) as ldir
	  from planet_osm_point
	  where amenity is not null or shop is not null
	) as pois
	

To use the `ldir` column in a stylesheet, set up a label style with the simple text placement type and nest some filters within that that adjust the `text-placements` parameter depending on the `ldir` value. This example will only try each label at one position:

接下来，在CartoCSS样式表中怎么使用`ldir`属性列呢？在将`text-placement-type`属性设置为`simple`之后，内嵌一组基于`ldir`值的过滤器以调整`text-placements`属性的值。在下面的样式表例子中，每个标注就只需要一个在一个位置尝试放置了。

	
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
	

After integrating this style into a more complete OSM stylesheet you can see that most of the point labels are now avoiding the roads.

将上面这段样式应用在一个完整的OSM数据样式表中之后，可以看到其中绝大部分的点标注都避开了道路网。

![](https://www.mapbox.com/tilemill/assets/pages/labels-ldir.png)





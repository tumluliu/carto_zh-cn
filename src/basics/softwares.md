### 支持CartoCSS的软件和系统

#### Mapbox Studio

Mapbox Studio的前身是TileMill，为Mapbox公司的开源地图制图软件，其制图所使用的脚本语言就是CartoCSS。它既有基于Web的在线应用，也有支持各种操作系统平台的客户端软件，基于node.js开发。围绕Mapbox Studio，还有一系列与CartoCSS相关的开源软件。更多信息请参考Mapbox Studio的[官方介绍](https://www.mapbox.com/mapbox-studio)（需要翻墙），以及Mapbox在github上的[开源项目](https://github.com/mapbox)（有时需要翻墙）。

#### CartoDB

[CartoDB](http://cartodb.com/)（需要翻墙）是一个以管理地理空间数据为主要目标，同时也可以将所管理的地理空间数据进行分析、制图渲染并发布的在线系统。从某种意义上说，CartoDB更像是一个GIS，而且是一个没有桌面客户端的在线WebGIS。与Mapbox一样，CartoDB也维护着一系列[开源项目](https://github.com/CartoDB)，而且CartoDB的在线应用本身就是开源的。

更酷的是，CartoDB通过其开源项目[torque](https://github.com/CartoDB/torque)（不是那个HPC资源管理软件[torque](http://www.adaptivecomputing.com/products/open-source/torque/)）实现了对动态过程的可交互式展示，而且动态过程的样式可以通过CartoCSS进行配置。关于torque的更多信息，请参考其[API文档](https://github.com/CartoDB/torque/blob/master/doc/API.md)。

#### higis

higis是一个利用高性能计算（High Performance Computing, HPC）平台实现对地理空间数据的高效管理、分析处理与制图可视化的地理信息系统。它的核心理念是利用HPC提供的大规模混合并行计算环境（多机、多核、众核）来提升地理空间信息管理、分析与可视化的效率。关于higis的更多特点，可参考发表于Geocomputation 2013会议上的文章[HiGIS: When GIS Meets HPC](http://www.geocomputation.org/2013/papers/26.pdf)。从技术体系上来说，higis受到了Mapbox与CartoDB中众多开源项目的启发，但也针对HPC环境进行了修改与定制。其在线制图功能采用CartoCSS对地图进行样式配置。

目前，higis在互联网上暂无可供展示其功能与能力的演示系统。但可以通过一些部署了higis的应用单位来一窥其样貌。这些单位有：中国国土资源部地质调查局某应用，上海交通大学OpenSAR处理平台，湖南省国土资源厅某应用。
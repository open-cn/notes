## OpenGIS
Open Geodata Interoperation Specification，简称OGIS
开放的地理数据互操作规范

OGIS是指在计算机和通信环境下，根据行业标准和接口所建立起来的地理信息系统。

OGIS由开放地理信息系统协会(Open GIS Consortium，简称OGC)提出。

目的是促进采用新的技术和商业方式来提高地理信息处理的互操作性(Interoperability)，它致力于消除地理信息应用（如地理信息系统，遥感，土地信息系统，自动制图/设施管理(AM/FM)系统）之间以及地理应用与其它信息技术应用之间的藩篱，建立一个无“边界”的、分布的、基于构件的地理数据互操作环境。




OpenGIS定义了一组基于数据的服务，而数据的基础是要素（Feature）。

所谓要素简单地说就是一个独立的对象，在地图中可能表现为一个多边形建筑物，在数据库中即一个独立的条目。要素具有两个必要的组成部分，几何信息和属性信息。

OpenGIS将几何信息分为点、边缘、面和几何集合四种：线（Linestring）属于边缘的一个子类，而多边形（Polygon）是面的一个子类。也就是说OpenGIS定义的几何类型并不仅仅是常见的点、线、多边形三种，它提供了更复杂更详细的定义，增强了未来的可扩展性。

另外，几何类型的设计中采用了组合模式（Composite），将几何集合（Geometry Collection）也定义为一种几何类型，类似地，要素集合（FeatureCollection）也是一种要素。

属性信息没有做太大的限制，可以在实际应用中结合具体的实现进行设置。 

相同的几何类型、属性类型的组合成为要素类型（FeatureType），要素类型相同的要素可以被存放在一个数据源中。而一个数据源只能拥有一个要素类型。因此，可以用要素类型来描述一组属性相似的要素。

在面向对象的模型中，完全可以把要素类型理解为一个类，而要素则是类的实例。 通过GIS中间件可以从数据源中取出数据，供WMS服务器和WFS服务器使用。WMS服务器接收请求，根据请求内容的不同，可以返回不同格式的最终数据。例如，WMS可以返回常用图片格式的地图片段供最终用户阅读（类似Google Maps），其中地图是根据一个样式文件（SLD）生成的，它描述了地图的线划粗细，色彩等；WMS也可以返回GeoRSS和KML用来和其它地图服务互通。WFS服务器也可以接收请求，但WFS将返回GML 格式的地理信息数据。GML是一种基于XML的数据格式，它可以完整的再现数据，也是 OpenGIS数据源的重要形式。也就是说，WFS返回的GML可以继续作为数据源。在WFS请求中，OpenGIS定义了一个Filter标准，用来实现对数据的筛选，使WFS更加灵活。另一方面，WFS还支持通过WFS-t提交客户端对数据的修改。通俗地说，WMS是“只读”的，而WFS则是可以读写的。 


### 软件标准

#### 软件类库

##### 几何基础类库

代表： JTS（Java）， GEOS（C++）， Shapely（Python）

##### 数据源实现

代表：PostGIS（PostgreSQL），MySQL Spatial

##### 中间件

代表：GeoTools（Java）中包括Filter、坐标转换、GML。

##### WMS/WFS服务器

代表： GeoServer（Java），MapServer（PHP）

##### 客户端

代表：OpenLayers/MapBuilder（JavaScript），uDig（Java），QGIS（C++）

#### Shapefile

ESRI的Shapefile格式是GIS矢量文件格式的事实标准，通常由.shp, .shx, .prj, .dbf等文件组成。OpenGIS的实现软件普遍支持Shapefile的读写。Shapefile在GeoServer中可以直接作为数据源，但是这种方式并不被推荐，原因很简单，基于文件的数据源可能造成性能不佳和数据丢失。

#### GML

GML是OpenGIS的标准规范之一，它基于xml描述地理数据。于Shapefile相比，xml更容易读写，易于在网络中以各种形式传播。同时，xml还具有可读性，人可以理解和辨识。GeoTools实现了GMLDataStore，因此在GeoServer中GML也可以直接作为数据源（需要下载GML扩展）。同时，GML的数据源为数据源动态化提供了实现的思路和可能性。

#### PostGIS

PostGIS是加拿大Refractions公司支持的开源项目，它为开源数据库PostgreSQL提供了空间支持。PostGIS安装后，PostgreSQL中出现一个模版数据库，新建空间数据库时只需以PostGIS为模版即可。PostGIS在SQL级别上实现了基本的空间运算功能。另外绝大多数开源GIS软件（即使是不严格遵守OpenGIS标准的）都支持PostGIS数据表的直接载入，读写等功能。毋庸置疑，PostGIS是OpenGIS数据源最佳实现。

#### MySQL Spatial

MySQL从MySQL4.0开始加入了Spatial扩展功能，实现了OpenGIS规定的几何数据类型，在SQL中的简单空间运算。但是从4.0之后到现在，MySQL的Spatial部分一直没有继续的更新和增强。加上早先MySQL在SQL上对空间运算支持的不完善（只支持基于最小外接矩形的关系判断），所以MySQL是开源数据源中一个不太让人满意的选择。不过由于MySQL在小型项目上的广泛引用，在一些情况下也是可以以MySQL为数据源的。


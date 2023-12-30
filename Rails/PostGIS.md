
## Spatial Database 

Spatial databases are simply normal databases that include support for several additional data types commonly used to represent location. These data types can include points on the surface of the earth (latitude and longitude) as well as lines, polygons, and other shapes. RGeo, together with the `activerecord-postgis-adapter`, lets you interact with these data types just like you would any other attribute on an ActiveRecord object.


## "WKT" (Well-Known Text)
is commonly used in spatial applications. In the WKT representation of a location, notice that the longitude comes first, and there is no comma between longitude and latitude
`"POINT(-122.193963 47.675086)"` 

## Querying by location
The real power of a spatial database such as PostGIS comes from its query capabilities. Spatial databases typically provide a rich set of SQL functions that you can use to build a wide variety of location-based queries.

## Spatial index 


## Spatial Data Types 
The spec SFS  defines a suite of seven concrete data types capable of representing points and piecewise linear objects in two-dimensional space, along with a set of standard operations that can be performed on them.

The seven data types defined by the SFS include three geometric types, and four collection types. They are as follows. 

#### **Point**.  
This is a simple point in two-dimensional space, identified by an x and y coordinate. Often, Points are used to represent locations on the surface of the earth, and sometimes (but not always) the x and y coordinate are interpreted as longitude and latitude, respectively. In other cases, a Point could simply represent a point on the X-Y plane.

#### **LineString**. 
This is a set of one or more straight line segments connected end to end. A common use for a LineString might be a set of driving directions. LineStrings may be self-intersecting, and some special LineStrings may be closed loops where the start point is the same as the end point.

![LineString](https://daniel-azuma.com/img/georails/georails-3-spec-linestrings.png) 


#### **Polygon**. 
This is a continguous area in the plane, with piecewise linear borders. Polygons can also have holes. A common use for a Polygon might be a city or country boundary. Below are a few examples of Polygons
![Polygon](https://daniel-azuma.com/img/georails/georails-3-spec-polygons.png)


> **For each of the above three types, there is a corresponding collection type that can represent zero or more of that type of object.**

- polygon can be constructed using linear ring, and linear linear ring can be constructed using 4 <= points  
- In the Well-Known Text (WKT) representation of a polygon, the first and last points of the linear ring must be the same to form a closed loop.


 **MultiPoint** may include zero or more Points, **MultiLineString** may include zero or more separate LineStrings, and **MultiPolygon** may include zero or more nonoverlapping Polygons.
 generic **GeometryCollection** type that may contain zero or more of any type of object, without any restrictions.

Some operation are defined (distance, intersection) for all types, other (Area) are specific to certain types. 

## GeneralizedGeoJSON - Specification

This is the specification for generalized GeoJSON-files!

This specification was established to agree on a uniform syntax for GeoJSON-files that are generated while testing and applying generalisation codes/algorithms!

## Base
Basically, this relies on the fundamental [GeoJSON-Specification](http://geojson.org/geojson-spec.html)

This specification consists of the following elements:

* **FeatureCollection**
	- this is the major object, which is necessary for all GeoJSON-parser
	- it contains only:
		- a type-definition: "type":"FeatureCollection"
		- a *JSON-object* for semantics: "properties":{} 
		- and an *array* of ancilliary objects: "features":[]
* **Feature**
	- this is the major object for kind all geometries
	- it contains:
		- a type-definition: "type":"Feature"
		- a *JSON-object* for semantics: "properties":{}
		- and a *JSON-object* for the geometrical information: "geometry":{}
* **Geometry**
	- this is the basic object for one geometry
	- it contains:
		- a type-definition: 
			* "type":"Point"
			* "type":"MultiPoint"
			* "type":"LineString"
			* "type":"MultiLineString"
			* "type":"Polygon"
			* "type":"MultiPolygon"
		- and the corresponding coordinates: "coordinates":[]
* **Coordinates**
	- this is always just an array, whose 'depth' is defined by the geomtry-type:
		* "Point" --> [x,y]
		* "MultiPoint"	--> [[Point1],...,[PointN]]
		* "LineString"	--> [[Point1],...,[PointN]]
		* "MultiLineString"	--> [[LineString1],...,[LineStringN]]
		* "Polygon"	--> [[LineString_outerRing],[LineString_innerRing1],...,[LineString_innerRingN]]
		* "MultiPolygon"	--> [[Polygon1],...[PolygonN]]

***!There is also a geometry-type "GeometryCollection"! Currently the GeneralizedGeoJSON-Specification does not include this geometry-type!!!***

## Specification

### Work with FeatureCollections
*We strongly recommend you to use this file-type! It is very flexible and GitHub can visualise your data directly!*

**Your initial file has to follow exactly the GeoJSON-Specifiaction, described above!**

A corresponding GeoJSON-file [(featColl.geojson)](featColl.geojson) would look this way:

```JavaScript
{
    "type": "FeatureCollection",
    "properties":{
    	"type":"original"
    }
    "features": [
        {
            "type": "Feature",
            "properties": {
                "type": "undefined",
                "name": "very short",
                "color": "#f00"
            },
            "geometry": {
                "type": "LineString", 
                "coordinates": [ [100.0, 0.0], [101.0, 1.0] ] 
            }
    ]
}
```

We offer you 2 basic possibilities for the generalized version of this file:
* store your resulting FeatureCollections in one file for each result ('multiple-files')
* store your resulting FeatureCollections in a superior FeatureCollection ('single-file')

#### Version1: 'multiple-files'

The syntax of each file is basically always the same.
You only have to define the generalisation-parameters within the properties-object of the FeatureCollection!

A corresponding GeoJSON-file [(featColl_multiple-files.geojson)](featColl_multiple-files.geojson) looks as follows:
```JavaScript
{
    "type": "FeatureCollection",
    "properties":{
    	"type":"generalized",
    	"generalization":{
    		"name":"algorithmName",
    		"parameter1":10,
    		"parameterN":10
    	}
    }
    "features": [
        {
            "type": "Feature",
            "properties": {
                "type": "undefined",
                "name": "very short",
                "color": "#f00"
            },
            "geometry": {
                "type": "LineString", 
                "coordinates": [ [100.0, 0.0], [101.0, 1.0] ] 
            }
    ]
}
```

***Have a look at the [exemplary file](featColl_example.geojson)!***

#### Version2: 'single-file'

The syntax of each ancilliary is exactly the same as for the previous Version1.
The only difference is, that you store them not in separate files, but directly within a GeoJSON-FeatureCollection!

A corresponding GeoJSON-file [(featColl_multiple-files.geojson)](featColl_multiple-files.geojson) looks as follows:
```JavaScript
{
    "type": "FeatureCollection",
    "properties":{
    	"type":"superior"
    }
    "features": [
		{
		    "type": "FeatureCollection",
		    "properties":{
		    	"type":"original"
		    },
		    "features": []
		},
		{
		    "type": "FeatureCollection",
		    "properties":{
		    	"type":"generalized",
		    	"generalization":{
		    		"name":"algorithmName",
		    		"parameter1":5,
		    		"parameterN":5
		    	}
		    },
		    "features": []
		},
		{
		    "type": "FeatureCollection",
		    "properties":{
		    	"type":"generalized",
		    	"generalization":{
		    		"name":"algorithmName",
		    		"parameter1":10,
		    		"parameterN":10
		    	}
		    },
		    "features": []
		}
    ]
}
```
***Have a look at the [exemplary files](single-file_example)!***

### Work with pure GeoJSON-geometries
*This is not recommended, especially because GitHub is not able to visualise these files*

You work directly with the GeoJSON-geometry-object (**"geometry":{}**)

A corresponging GeoJSON-file [(pure.geojson)](pure.geojson) would look this way:

```JavaScript
{"type": "LineString", "coordinates": [ [100.0, 0.0], [101.0, 1.0] ] }
```
*You can see, that this is only the pure geometry! So you don't have the "properties"-object and therefore you cannot add any information on the generalisation, like name of algorithm or values for parameter.*

That is why the [corresponging GeoJSON-file](pure.geojson) has to contain these information within the file name:

```shell
pure_algorithmName_parameter1_paramterN.geojson
```
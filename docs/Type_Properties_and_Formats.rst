==============================
Type Properties and Formats
==============================

The following keywords are used to the define the ``properties`` in the Type defintion:

=================== ============================= 	
Name                Value							
=================== =============================	
``type``                Required type of the Property which must match a ``type`` listed in the Supported Formats table below.
``format``              Optional format of the Type Propety that, if specified, must be from the table below.
``reftype``             Optional ``id`` to a previously defined Type. When specified, ``type`` must be set to ``object``. Defines an aggregate relationship to a previously defined Type. 
``includetype``         Optional ``id`` to a previously defined Type. When specified, ``type`` must be set to ``object``. Defines a composition relationship to a previously defined Type. 
``typeversion``			Optional version set when using ``reftype`` or ``includetype``. The version must be of format x.x.x.x, where x must be an integer greater than or equal to 0. If omitted, the current version of the specified Type will be used.
``isindex``   	        At least one Type Property must be designated as the index by supplying the ``isindex`` keyword with a value of true. The designated isindex property is used to uniquely identify discrete Data objects so that they can be updated or deleted after their initial creation. For a compound index, the order of index properties within the message determines the order within the index.
``isname``              One Type Property may optionally be designated as the name by supplying the ``isname`` keyword with a value of true. Because the index must be unique across all Data objects, the isname keyword allows for multiple distinct Data objects to share a common name.
``isquality``			One Type Property may optionally be designated as the Quality by supplying the ``isquality`` keyword with a value of true. The designated isquality property is used to determine if the collection of data values has the status `good`, `bad` or `questionable`.
``name``                Optional friendly name for the Type Property. This property can be overridden using the propertyoverrides keyword on a Container message.
``description``         Optional description for the Type Property. This property can be overridden using the propertyoverrides keyword on a Container message.
``uom``					Optional unit of measure for the Type Property. This property can be overridden using the propertyoverrides keyword on a Container message.
``minimum``				Optional type qualifier that defines minimum allowed value. This property can be overridden using the propertyoverrides keyword on a Container message.
``maximum``				Optional type qualifier that defines maximum allowed value. This property can be overridden using the propertyoverrides keyword on a Container message.
``interpolation``		Optional data mode used to provide consistency when reading data. Supported values include ``continuous``, ``discrete``, ``stepwisecontinuousleading``, and ``stepwisecontinuousfollowing``.
``extrapolation``		Optional data mode used to provide consistency when reading values. Supported values include ``all``, ``none``, ``forward``, and ``backward``.
=================== =============================	

The ``type`` property must be set to a valid JSON type as defined by JSON Schema. The supported types include string, integer, number, object, boolean, and array. 
The format of the ``type`` may be set using the ``format`` keyword, as described in the Supported Formats table below. This allows for the creation of timestamps, dictionaries, and bit length-specific numeric properties.

Types can be composed from other Types using ``reftype`` or ``includetype`` on a Property definition. This provides the ability to define an aggregate or compositional relationship between Type definitions at the property level.
When relating ``static`` and ``dynamic`` data, a ``reftype`` property would be used to provide a placeholder in the ``static`` type for the ``dynamic`` data.
When relating data that uses a compositional relationship and encapsulates the properties in the new Type, define a property to be an ``includetype``.

  
Supported Formats
-----------------

========   =================  	======================  ===========
Type       Format             	Default Value           Description
========   =================	======================  ===========
array                           null                    An array of objects. The required ``items`` keyword defines the type of the objects in the array.                           
boolean                         false                   A value of either "true" or "false".
integer    int64                0                       64-bit integer.
integer    int32 (default)      0                       32-bit integer.
integer    int16                0                       16-bit integer.
integer    uint64               0                       64-bit unsigned integer.
integer    uint32               0                       32-bit unsigned integer.
integer    uint16               0                       16-bit unsigned integer.
number     float64              0                       64-bit floating point.
number     float32 (default)    0                       32-bit floating point.
number     float16              0                       16-bit floating point.
object     dictionary           null                    A dictionary of objects, indexed by a string key. The ``additionalProperties`` keyword defines the dictionary's value type.                             
string                          null                    A string.
string     date-time            0001-01-01T00:00:00Z    A string representation of a timestamp, formatted as YYYY-MM-DDThh:mm:ssZ, with optional subsecond precision.                        
========   =================    ======================  ===========



Nullable type properties: 

Nullable type properties are supported by specifying an array that defines the datatype and includes the keyword ``null``. 
The data type and ``null`` may appear in any order in the array. For example: 

::

	"MeasurementValue": {"type": ["integer", "null"], "format": "int64"}
	
::

	"MeasurementTimestamp": {"type": ["null", "string"], "format": "date-time"}
	

	
Complex Types, Inheritance and Type Reuse:

Complex types, Inheritance and Type reuse are supported by specifying either ``reftype`` or ``includetype`` on a Property, or ``basetype`` on a Type message. The values are referenced by using the ``id`` of the previously defined Type. 
If the referenced type has multiple versions the ``typeversion`` may be set to target a particular version of the Type. The version can be specified when using ``reftype``, ``includetype``, or ``basetype`` as needed.

For inheritance, ``basetype`` would be used to include previously defined properties and create a flat referencing structure. This should be used to relate ``static`` to ``static``, or ``dynamic`` to ``dynamic``.
For example the Type 'TruckProperties' will contain four properties:  Latitude, Longitude, Timestamp, and Speed.

::

	{ 	"id": "Location",
		"type": "object",
		"version": "1.0.0.0",
		"classification":"dynamic",
		"properties": { 
			"Latitude":{ "type":"integer", "format":"int64"}
			"Longitude":{ "type":"integer", "format":"int64"},
			"Timestamp":{ "type":"string", "format":"date-time", "isindex":true}
		}
	}, {
		"id":"TruckProperties",
		"type":"object",
		"classification":"dynamic",		
		"basetype": [ { "typeid":"Location", "typeversion":"1.0.0.0" } ],		
		"properties": { 
			"Speed":{ "type":"integer", "format":"int16" }
		}
	}

For complex types that relates ``static`` and ``dynamic`` data, providing a placeholder for the ``dynamic`` data, use ``reftype`` when defining the relationship. 
For example, the new 'Tank' Type will contain the properties: TankName, Pressure, Temperature, and includes the timestamp for the dynamic data. 

::

	{ 	"id":"TankMeasurement",
		"type":"object",
		"classification":"dynamic",
		"properties": { 
			"Pressure":{ "type":"number", "format":"float32"},
			"Temperature":{ "type":"number", "format":"float32"},
			"Timestamp":{ "type":"string", "format":"date-time", "isindex":true}
		}
	}, {
		"id":"Tank",
		"type":"object",
		"classification":"static",
		"properties": { 
			"TankName":{ "type":"string", "isindex":true, "isname":true }
			"Measurements": { "type":"object", "reftype":"TankMeasurement", "typeversion":"1.0.0.0" }	
		}
	}
	

For types that need to include properties from a previously defined type within a Property, use the ``includetype`` when defining the relationship. 
For example, the new 'Tankv4' Type will contain the properties: TankName, Location.Latitude, Location.Longitude, slong with the Location timestamp. 

::

	{ 	"id":"LocationProperties",
		"type":"object",
		"classification":"static",
		"properties": { 
			"Latitude":{ "type":"number", "format":"float32", "isindex":true },
			"Longitude":{ "type":"number", "format":"float32", "isindex":true }
		}
	}, {
		"id":"Tankv4",
		"type":"object",
		"classification":"static",
		"properties": { 
			"TankName":{ "type":"string", "isindex":true }
			"Location": { "type":"object", "includetype":"LocationProperties", "typeversion":"1.0.0.0" }	
		}
	}
	
Type Qualifiers:

Properties with ``isindex`` keyword designate that property as the index and must have unique values. The index value is set when creating instances of the Type.
Properties of a ``dynamic`` type with the ``isindex`` keyword are typically used for indexing on time, and in that case the ``type`` is ``string`` and the ``format`` is ``date-time``. 

The ``isquality`` keyword is used to designate a particular property as the overall data quality for a Type. Properties of type boolean, integer, or a reference to an ``enum`` may be marked as supporting quality.  
For booleans, a value of false indicates a good value. For integers, a value of 0 indicates a good value, negative values indicate bad values, and positive values indicate questionable values. 
The quality of ``enum`` values is indicated in their type definition. Refer to the :doc:`Enum Type<Enum_Type>` for additional information about defining enums. The following formats are supported:
	
::

	"Quality": { "type":"boolean", "isquality": true }
	
::

	"Quality": { "type":"integer", "isquality": true }

::

	"Quality": { "type":"object", "reftype":"DataQuality", "isquality": true }	
   
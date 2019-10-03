Enum Types
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

An ``enum`` Type can be defined using the ``classification`` enum on a Type message and specifies a set of allowed values for a Property. The enum Type is created separately so that it can later be referenced by properties in multiple type definitions. 
Use the properties to define the name/value pairs of the enumeration. 

An ``enum`` Type can also be used to define data quality values for systems that have more complex quality states. When defining data quality include the ``quality`` keyword in the property definition. 
Specify the status `good`, `bad`, or `questionable` for each ``value`` in the enum.  

For each property in the enumeration, include the following keywords. If a keyword is not specified, then the default value will be used.  
Similiar to Type definitions for static or dynamic data, the ``type`` property would also be included.

=================== =============================
Name                Value
=================== =============================
``value``			A unique numeric value associated with the enum. If not defined the enum ranges from 0 to one less than size of the collection.
``quality``			Optional field needed when defining an enum that holds data quality. Must be set to `good`, `bad`, or `questionable`. Defaults to 'good' when not specified.  
=================== =============================


For example, the possible states of a valve could be CLOSED or OPEN.  

::

	{ 
		"id": "ValveStateEnum", 
		"type": "object", 
		"classification":"enum",
		"properties": { 
			"CLOSED": { 
				"type":"integer"
				"value": 0				
			}, 
			"OPEN": {
				"type":"integer"
				"value": 1
			} 
		} 
	}
	

An ``enum`` could be used to define a dataset that represents data quality. Include the ``quality`` keyword in the property definitions. If ``quality`` is not explicitly set then it is assumed to be 'good'. 

::

	{ 
		"id": "DataQualityEnum", 
		"type": "object", 
		"classification":"enum",
		"properties": { 
			"Device Connected": { 
				"type":"integer"
				"value": 0	,
				"quality": "good"				
			}, 
			"Device Failure": {
				"type":"integer"
				"value": 1,
				 "quality": "bad"
			},
			"Device Comm Failure": { 
				"type":"integer"
				"value": 2	,
				"quality": "bad"				
			}, 
			"Uncertain - Out Limits": {
				"type":"integer"
				"value": 3,
				"quality": "questionable"
			} 
		} 
	}


When referencing an ``enum`` for a property that holds quality, include the `isquality` keyword and use ``reftype`` as the data type of the property:

::

	{ "DataQuality": { "type": "object", "reftype": "DataQualityEnum", "isquality": true } }




Examples:

::
	
	[{ 
		"id": "ValveStateEnum", 
		"type": "object", 
		"classification":"enum",
		"properties": { 
			"CLOSED": { 
				"type":"integer"
				"value": 0				
			}, 
			"OPEN": {
				"type":"integer"
				"value": 1
			} 
		} 
	}, {	
		"id": "TankMeasurementv2",
		"version": "1.0.0.0",
		"type": "object",
		"classification": "dynamic",
		"properties": {	
			"Timestamp": {                        
				"type": "string", 
				"format":"date-time",
				"isindex": true		
			},
			"ValvePosition": {			
				"type": "object",
				"reftype": "ValveStateEnum"
			}			
			"Pressure": {
				"type": "number",
				"name": "Tank Pressure",
				"description": "Tank Pressure in Pa",
				"uom": "pascal"
			},
			"Temperature": {
				"type": "number",
				"name": "Tank Temperature",
				"description": "Tank Temperature in K",
				"uom": "K" 				
			}						
		}
	}]
	
	
::
	
	[{ 
		"id": "DataQualityEnum", 
		"type": "object", 
		"classification":"enum",
		"properties": { 
			"Device Connected": { 
				"type":"integer"
				"value": 0	,
				"quality": "good"				
			}, 
			"Device Failure": {
				"type":"integer"
				"value": 1,
				 "quality": "bad"
			},
			"Device Comm Failure": { 
				"type":"integer"
				"value": 2	,
				"quality": "bad"				
			}, 
			"Uncertain - Out Limits": {
				"type":"integer"
				"value": 3,
				"quality": "questionable"
			} 
		} 
	}, {	
		"id": "TankMeasurementv3",
		"version": "1.0.0.0",
		"type": "object",
		"classification": "dynamic",
		"properties": {			
			"Pressure": {
				"type": "number",
				"name": "Tank Pressure",
				"description": "Tank Pressure in Pa",
				"uom": "pascal"
			},
			"Temperature": {
				"type": "number",
				"name": "Tank Temperature",
				"description": "Tank Temperature in K",
				"uom": "K" 				
			},
			"Timestamp": {                        
				"type": "string", 
				"format":"date-time",
				"isindex": true		
			},
			"Quality": {
				"type": "object",
				"reftype": "DataQualityEnum", 
				"isquality": true
			}
		}
	}]
	
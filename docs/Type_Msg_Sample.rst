Type Example 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In the following example we create 2 ``static`` types and a ``dynamic`` type. The Type definitions define properties for the ``isindex`` and ``isname`` qualifiers. Their values will be referenced later when defining instance data and creating relationships.


**Headers**
::

	omfversion = 1.2
	messagetype = type
	action = create
	messageformat = json


**Body**
::

	[{
		"id": "Plant",
		"version": "1.0.0.0",
		"type": "object",
		"classification": "static",
		"properties": {
			"PlantId": {
				"type": "string",
				"isindex": true
			},
			"PlantName": {
				"type": "string",
				"isname": true
			},
			"Address": {
				"type": "string"
			},
			"Contact": {
				"type": "string"
			}
		}
	}, {
		"id": "Tank",
		"version": "1.0.0.0",
		"type": "object",
		"classification": "static",		
		"properties": {
			"TankName": {
				"type": "string",
				"isindex": true,
				"isname": true				
			},
			"Serial": {
				"type": "string"
			},
			"Model": {
				"type": "string"
			}
		}
	}, {
		"id": "TankMeasurement",
		"version": "1.0.0.0",
		"type": "object",
		"classification": "dynamic",		
		"properties": {	
			"Timestamp": {                        
				"type": "string", 
				"format":"date-time",
				"isindex": true		
			}		
			"Pressure": {
				"type": "number",
				"name": "Tank Pressure",
				"description": "Tank Pressure in Pa",
				"uom": "pascal",
				"interpolation": "Continuous",
				"extrapolation": "Forward",
				"max": "20",
				"min": "10"
			},
			"Temperature": {
				"type": "number",
				"name": "Tank Temperature",
				"description": "Tank Temperature in K",
				"uom": "K" 				
			}			
		}
	}]



Properties of a Type definition can reference a previously defined Type using the ``reftype`` keyword, relating ``static`` and ``dynamic`` data in a single Type definiton. 
In this example we reference the previously defined Type 'TankMeasurement' in the Type 'Tankv2':

::

	[{
		"id": "Tankv2",
		"version": "1.0.0.0",
		"type": "object",
		"classification": "static",		
		"properties": {				
			"TankName": {
				"type": "string",
				"isindex": true,
				"isname": true				
			},
			"Serial": {
				"type": "string"
			},
			"Model": {
				"type": "string"
			}
			"Measurements": {
				"type":"object",
				"reftype":"TankMeasurement"	
			}		
		}
	}]
	
Properties of a Type definition can reference a previously defined Type using the ``basetype`` keyword, inheriting ``static`` data from another ``static`` Type. 
In this example we reference the previously defined Type 'Tankv2'. 

::

	[{
		"id":"RectangularTank",
		"version": "1.0.0.0",
		"type": "object",
		"classification": "static",
		"basetype": [ "Tankv2" ],
		"properties": {						
			"TankHeight": {
				"type": "integer",
				"format":"int32",
				"uom":"ft"				
			},
			"TankWidth": {
				"type": "integer",
				"format":"int32",
				"uom":"ft"
			}			
		}
	}, {
		"id":"CylindricalTank",
		"version": "1.0.0.0",
		"type": "object",
		"classification": "static",
		"basetype": [ "Tankv2" ],
		"properties": {								
			"TankDiameter": {
				"type": "number",
				"format":"float32"		
			}
		}
	}]


Properties of a Type definition can reference a previously defined Type using the ``includetype`` and encapsulate the properties in the new Type. 
In this example we include the 'FacilityClassification' in the Type Plant.

::

	[{ 	"id":"FacilityClassification",
		"type":"object",
		"classification":"static",
		"properties": { 
			"FacilityType":{ "type":"string", },
			"ClassificationID":{ "type":"string", "isindex":true }
		}
	}, {
		"id":"Plantv2",
		"type":"object",
		"classification":"static",
		"properties": { 
			"PlantName":{ "type":"string", "isname":true },
			"PlantID":{ "type":"string", "isname":true },
			"Address": { "type": "string" },
			"Contact": { "type": "string" },
			"Classification": { "type":"object", "includetype":"FacilityClassification" }	
		}
	}]


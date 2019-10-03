Container Example
^^^^^^^^^^^^^^^^^^

**Headers**

::
	
	omfversion = 1.2
	messagetype = container
	action = create
	messageformat = json


**Body**

::

	[{
		"id": "Tank1Measurements",
		"typeid": "TankMeasurement",
		"datasource":"Modbus",
		"indexes": ["Pressure"]			
	}, {
		"id": "Tank2Measurements",
		"typeid": "TankMeasurement",
		"datasource":"Modbus",				
		"propertyoverrides": {
			"Temperature{				
				"description": "Tank Temperature in degree Fahrenheit",
				"uom": "F"
			}
		}			
	}, {
		"id": "Tank_R1_Measurements",
		"typeid": "TankMeasurement",
		"datasource":"Modbus"
	}]


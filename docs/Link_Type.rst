Link Data Messages
^^^^^^^^^^^^^^^^^^^^^

A Link is a pre-defined Type with id ``__Link``, and can be used to create relationships between instances of static data, or between instances of static and dynamic data. 

A ``Link`` message contains the following properties: 

=================== ==================================================
Name                Value
=================== ==================================================
``source``			An object representing the source of the link.
``target``			An object representing the target of the link.
=================== ==================================================

Each ``source`` and ``target`` object has the following keywords:

=================== ======================================
Name                Value
=================== ======================================
``typeid``          Optional id of the type. If omitted, containerid is expected.
``containerid``     Optional id of the container. If omitted, typeid is expected.
``typeversion``     Optional version of the type to be linked to or from. The version must be of format x.x.x.x, where x must be an integer greater than or equal to 0. If omitted version 1.0.0.0 is assumed.
``index``           Value of the :doc:`isindex <Type_Properties_and_Formats>` property defined when creating the instance of a Type. If typeid is specified, index is required. If containerid is specified index is not supported.
``property``		Optional name of a property defined in the Type definition to be used by the link relationship.
=================== ======================================

If the relationship is established when the Type is defined using ``reftype``, ``includetype`` or ``basetype`` then an explicit ``__Link`` message is not required within the Data message. 
The relationship has been previously established and the instance data can be directly accessed.	
If the relationship is not known at the time when the Type is created, then a link can be used to define the relationship.

To relate an instance of a type, specify the ``typeid`` and ``index`` of the instance.

To relate to a container, specify only the ``containerid``.

For example, to relate an instance of a Plant, specified by its indexed property PlantId with value WTP1, to an instance of Tank, specified with by its indexed property TankName with value Tank1, use the following __Link:

::

	[{ 
		"typeid": "Plant", 
		"values": [{ "PlantId": "WTP1", "PlantName": "Water Treatment Plant One" }] 
	}, { 
		"typeid": "Tank", 
		"values": [{ "TankName": "Tank1" }] 
	}, { 
		"typeid": "__Link", 
		"values": [{ "source": { "typeid": "Plant", "index": "WTP1"}, 
			"target": { "typeid": "Tank", "index": "Tank1" } }]
	}]
	

To associate the container, Tank1Measurements, with the instance of a Tank whose index is Tank1, use the following __Link:

::

	[{ 	"typeid": "__Link", 
		"values": [{ "source": { "typeid": "Tank", "index": "Tank1"}, 
			"target": {"containerid": "Tank1Measurements"} }]
	}]	



If a Type message includes the ``dynamic`` data using the ``reftype`` then the explicit ``__Link`` message is not needed, and the ``containerid`` can be included when creating the Type instance.
For example, if the Tank contains a property TankMeasurements as ``reftype``, and TankMeasurements is ``dynamic`` then the relationship is established when the Tank instance is created.

::

	{
		"typeid": "Tank",
		"values": [{ 
			"TankName": "Tank2", 
			"Serial": "2364-4243-FS12", 
			"Model": "TK-421",
			"Measurements": { 
				"containerid": "Tank2Measurements" 
			} 
		}]
	}

If a Type message includes the related data using the ``basetype`` then the explicit ``__Link`` message is not needed, and the ``basetype`` properties are directly referenced when creating the instance.
For example, if the RectangularTank contains the ``basetype`` Tank, then an instance of a RectangularTank includes the Tank properties TankName, Serial, Model, and Measurements, along with the RectangularTank properties. 

::

	{
		"typeid":"RectangularTank",		
		"values": [{ 
			"TankName": "Tank_R1",
			"Serial":"4687-3278-CEB8",
			"Model":"KD-2264"
			"Measurements": {
				"containerid": "Tank_R1_Measurements"
			},
			"TankHeight": 120,
			"TankWidth": 90
		}]
	}
	
If the same Type message had been created with the ``includetype`` then the explicit ``__Link`` message is not needed, and the properties are referenced using the ``includetype`` Property.
For example, if the RectangularTank references a Tank Type using ``includetype``, the Tank properties are referenced using the property TankInformation. 

::

	{
		"typeid":"RectangularTank",		
		"values": [{ 
			"TankInformation": {
				"TankName": "Tank_R1",
				"Serial":"4687-3278-CEB8",
				"Model":"KD-2264"
				"Measurements": {
					"containerid": "Tank_R1_Measurements"
				},
			}
			"TankHeight": 120,
			"TankWidth": 90
		}]
	}
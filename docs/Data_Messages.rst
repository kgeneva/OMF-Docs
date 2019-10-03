Data Messages
-------------

Data messages can span multiple Types and Containers. The body of a Data message is composed of a JSON array with JSON objects defined by the following keywords: 

=============== =================================================================================================
Name            Value
=============== =================================================================================================
``typeid``      Optional ID of the type. If omitted, container is expected.
``containerid`` Optional ID of the container. If omitted, type is expected.
``typeversion`` Optional version of the Type, if one is specified. The version must be of format x.x.x.x, where x must be an integer greater than or equal to 0. If omitted, version 1.0.0.0 is assumed.
``values`` 	    An array of objects conforming to the type.
=============== =================================================================================================

For each object, either ``typeid`` or ``containerid`` must be specified. If ``containerid`` is specified, the values must conform to the Type with which the Container is associated. If ``typeid`` is specified, the values must conform to that Type.

If a Type Property is defined but no property value is provided in the Data message, a default value will be assumed. Default values are specified in :doc:`Type_Properties_and_Formats`.

If a Type is defined using ``basetype``, ``reftype``, or ``includetype`` the following Data messsage formats are supported, and explicit link message are not needed.

If a Type references another Type using ``basetype``, both Types are of the same classification, and the inherited properties are included in the Data message.

If a Type references another Type using the ``reftype`` option on a Property, then the properties are referenced in the Data message using the ``reftype`` Property.
The same holds true if the ``includetype`` property was used to relate the Types.

In this example, TankName, Serial, Model and Measurements were included using ``basetype``. Measurements were included by ``reftype``. 

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
	}, {
		"containerid":"Tank_R1_Measurements",		
		"values": [{ 
			"Pressure": 36.320961,
			"Temperature": -82.341759,
			"Timestamp": 2019-09-11T23:01:12.430Z
		}]
	}


In this example, FacilityType and ClassificationID were included using ``includetype``. 

::

	{
		"typeid":"Plantv2",		
		"values": [{ 
			"PlantName": "Water Treatment Plant Two",
			"PlantID": "WTP2",
			"Address": "636 Holston Drive",			
			"Contact": "John Smith",
			"Classification": {
				"FacilityType": "Filtration Water Treatment Plant"
				"ClassificationID": "WTPFILTER2"
			}
		}]
	}

	
Linking instances of Types and Containers using Data messages:

:doc:`__Link<Link_Type>` messages are used to create relationships between instances of types, or instances of a type and container. To define a link relationship, set the ``typeid`` of the data message to ``__Link``. 
In the ``values`` array, define the ``source`` and ``target`` of the link relationship using the format defined by :doc:`Link Type<Link_Type>`


For examples of Linking Data, and examples of Data Messages see the following Topics:

.. toctree::

	Link_Type   	
	Data_Msg_Sample

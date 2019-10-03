===============
 Type Messages
===============
Types can be created, updated, or deleted. Updating a Type without updating the Type’s version will result in a failure. Once a Type is deleted, any operations on Containers or Data using that Type will fail.

The body of a Type message consists of an array of objects. The following keywords are used to define a Type:

=================== =============================
Name                Value
=================== =============================
``id``   	        Unique identifier of the Type.
``type``            Inherited from JSON Schema. Must be set to ``object``.
``version``         Optional version of the Type. The version must be of format x.x.x.x, where x must be an integer greater than or equal to 0. If omitted version 1.0.0.0 is assumed.
``classification``  One of ``dynamic``, ``static`` or ``enum``.
``basetype``		Optional array of Type ids and versions used to inherit properties from previously defined Types of the same ``classification``, supporting ``static`` or ``dynamic`` Type reuse. 
``name``            Optional friendly name for the Type.
``description``     Optional description for the Type.
``tags``            Optional array of strings to tag the Type.
``metadata``        Optional key-value pairs associated with the Type.
``properties``      Key-value pairs defining the properties of the Type.
=================== =============================

The ``id`` cannot begin with the character sequence __. This has been reserved for predefined message definitions. Currently the only supported predefined Type is :doc:`__Link<Link_Type>`.
The ``id`` property is referenced when creating instances of Types in Container and Data messages, or when creating other Types that include a reference to this Type.   

A ``static`` classification represents metadata describing a device being observed and should be used to capture data that is descriptive and relatively unchanging.
A ``dynamic`` classification represents observed or calculated measurements taken from a device. 
An ``enum`` classification represents a predefined set of allowed values and can be referenced by other properties and Types. Refer to the :doc:`Enum Type Message <Enum_Type>` for additional information on how to define and use an ``enum``.

The ``basetype`` keyword is used to support Type inheritance and is an array of previously defined Types and their version. When set the properties of those Types are included in this new Type.
To specify a version on the referenced Type, then include the typeversion in the ``basetype`` array:

::

	“basetype”: [ {“id”:”Tank”, “typeversion”:”1.2.0.0”} ]
	

``enum`` classification cannot be used in an inheritance relationship.


The format of the ``properties``, and the allowed data types is defined in the :doc:`Type Properties and Formats <Type_Properties_and_Formats>` Topic.	
For examples of Type messages and formatting refer to the following Topics:

.. toctree::

	Type_Properties_and_Formats	
	Type_Msg_Sample	
	Enum_Type	

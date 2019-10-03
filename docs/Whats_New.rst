What's New
==========

Version 1.2 introduces the following incremental changes from version 1.1:

Enhancements for Type messages:
	- Ability to create complex Types that relate ``static`` and ``dynamic`` data using ``reftype`` defined in the :doc:`Type Properties and Formats <Type_Properties_and_Formats>`. 
	- Ability to support inheritance for Types of the same ``classification`` using the ``basetype`` keyword defined in the :doc:`Type Message<Type_Messages>`. 
	- Ability to support composition of previously defined Types using ``includetype`` defined in the :doc:`Type Properties and Formats <Type_Properties_and_Formats>`. 
	- Support for creating reusable enumerations using a new ``classification`` on the Type message called ``enum``, and referencing the Type from a Property using :doc:`reftype <Type_Properties_and_Formats>`.   	
	- Ability to indicate if a data value is `good`, `bad`, or `questionable` using ``enum`` classification combined with the ``isquality`` keyword on the :doc:`Property <Type_Properties_and_Formats>`. 
	- Support for ``minimum`` and ``maximum`` type qualifiers defined on the :doc:`Property <Type_Properties_and_Formats>`, used to restrict data values.
	- Support for ``interpolation`` and ``extrapolation`` modes defined on the :doc:`Property <Type_Properties_and_Formats>`, used to categorize the type of data being stored.


Enhancements for Container messages:
	- Ability to override values of ``properties`` defined by a dynamic Type using the ``propertyoverrides`` keyword in the :doc:`Container Message <Container_Messages>`. Currently supported property overrides include ``name``, ``description``, ``uom``, ``minimum``, and ``maximum``.
	- Ability to set the source of a stream of data using the ``datasource`` keyword defined on a :doc:`Container <Container_Messages>`.


Enhancements for Data messages:
	- Ability to relate instances of static and dynamic data, or data of the same classification, without explicitly specifying a :doc:`__Link<Link_Type>` message when ``reftype``, ``basetype`` or ``includetype`` have been specified in the Type definition. 

	
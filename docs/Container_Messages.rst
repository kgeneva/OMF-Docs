Container Messages
------------------

A Container can be defined for Types whose classification is ``dynamic``, and provides a stream of data events. Each Container has a unique ID defined by the user.

Immediately after a Type has been registered using a Type message, Containers may be created using that Type. Containers support the same set of actions defined for Type messsages: ``create``, ``update``, and ``delete``.

The body of a Container message consists of an array of objects with the following keywords:

=======================	==================================================================================
Name					Value
=======================	==================================================================================
``id``          		Unique identifier of the Container.
``typeid``        		ID of the Type used by the Container.
``typeversion`` 		Optional version of the Type used by the Container. The version must be of format x.x.x.x, where x must be an integer greater than or equal to 0. If omitted, version 1.0.0.0 is used.
``name`` 	    		Optional friendly name for the Container.
``description`` 		Optional description for the Container.
``datasource``			Optional way to specify the source of a stream of data 
``tags``        		Optional array of strings to tag the Container.
``metadata``    		Optional key-value pairs associated with the Container. 
``indexes``     		Optional array of Type Property ids to be used as secondary indexes for the Container.
``propertyoverrides``	Optional key-value pairs used to override properties on a Type definition. 
=======================	==================================================================================

Certain keywords defined on Type ``properties`` may be overridden at the container level using the optional ``propertyoverrides`` keyword. Currently supported overrides include ``name``, ``description``, ``uom``, ``minimum``, and ``maximum``. 
Refer to the :doc:`Type Properties and Formats <Type_Properties_and_Formats>` for a complete list of Type ``properties``. 




.. toctree::

      Container_Msg_Sample

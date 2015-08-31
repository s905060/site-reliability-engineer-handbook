# RESTful API HTTP methods


### Applied to web services[edit]
Web service APIs that adhere to the REST architectural constraints are called RESTful APIs. HTTP-based RESTful APIs are defined with these aspects:
base URI, such as http://example.com/resources/
an Internet media type for the data. This is often JSON but can be any other valid Internet media type (e.g., XML, Atom, microformats, images, etc.)
standard HTTP methods (e.g., GET, PUT, POST, or DELETE)
hypertext links to reference state
hypertext links to reference-related resources[8]

|Resource	|GET	|PUT	|POST	|DELETE
|--|--|--|--|--
|Collection URI, such as http://api.example.com/v1/resources/	|List the URIs and perhaps other details of the collection's members.	|Replace the entire collection with another collection.	|Create a new entry in the collection. The new entry's URI is assigned automatically and is usually returned by the operation.[9]	|Delete the entire collection.
|Element URI, such as http://api.example.com/v1/resources/item17	|Retrieve a representation of the addressed member of the collection, expressed in an appropriate Internet media type.	|Replace the addressed member of the collection, or if it does not exist, create it.	|Not generally used. Treat the addressed member as a collection in its own right and create a new entry in it.[9]	|Delete the addressed member of the collection.

The PUT and DELETE methods are referred to as idempotent, meaning that the operation will produce the same result no matter how many times it is repeated. The GET method is a safe method (or nullipotent), meaning that calling it produces no side-effects. In other words, retrieving or accessing a record does not change it.

Unlike SOAP-based web services, there is no "official" standard for RESTful web APIs.[10] This is because REST is an architectural style, while SOAP is a protocol. Even though REST is not a standard per se, most RESTful implementations make use of standards such as HTTP, URI, JSON, and XML.[10]
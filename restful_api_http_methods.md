# RESTful API HTTP methods


Applied to web services[edit]
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

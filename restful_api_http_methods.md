# RESTful API HTTP methods

|Resource	|GET	|PUT	|POST	|DELETE
|--|--|--|--|--
|Collection URI, such as http://api.example.com/v1/resources/	|List the URIs and perhaps other details of the collection's members.	|Replace the entire collection with another collection.	|Create a new entry in the collection. The new entry's URI is assigned automatically and is usually returned by the operation.[9]	|Delete the entire collection.
|Element URI, such as http://api.example.com/v1/resources/item17	|Retrieve a representation of the addressed member of the collection, expressed in an appropriate Internet media type.	|Replace the addressed member of the collection, or if it does not exist, create it.	|Not generally used. Treat the addressed member as a collection in its own right and create a new entry in it.[9]	|Delete the addressed member of the collection.

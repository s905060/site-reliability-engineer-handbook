# Elasticsearch

CRUD
While we may want to use ElasticSearch primarily for searching the first step is to populate an index with some data, meaning the "Create" of CRUD, or rather, "indexing". While we're at it we'll also look at how to update, read and delete individual documents.

Indexing
In ElasticSearch indexing corresponds to both "Create" and "Update" in CRUD - if we index a document with a given type and ID that doesn't already exists it's inserted. If a document with the same type and ID already exists it's overwritten.

In order to index a first JSON object we make a PUT request to the REST API to a URL made up of the index name, type name and ID. That is: http://localhost:9200/<index>/<type>/[<id>].

Index and type are required while the id part is optional. If we don't specify an ID ElasticSearch will generate one for us. However, if we don't specify an id we should use POST instead of PUT.

The index name is arbitrary. If there isn't an index with that name on the server already one will be created using default configuration.

As for the type name it too is arbitrary. It serves several purposes, including:

Each type has its own ID space.
Different types can have different mappings ("schema" that defines how properties/fields should be indexed).
Although it's possible, and common, to search over multiple types, it's easy to search only for one or more specific type(s).
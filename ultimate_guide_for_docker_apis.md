# Ultimate Guide for Docker APIs

Throughout our Docker Tutorial Series, we have discussed many significant Docker components and commands. In today’s series installment, we dig deeper into Docker and uncover Docker APIs.

The first item worth noting is that Docker provides the following APIs, making it easier to use. Those APIs also come in four flavours:

* Docker Registry API
* Docker Hub API
* Docker OAuth API
* Docker Remote API

Specifically to this post, let’s discuss Docker Registry API and Docker Hub API.

### Docker Registry API

Docker Registry API is a REST API for the Docker Registry, which eases the storage of images and repositories. The API does not have access to user accounts or its authorization. Read part four of the Docker Tutorial Series to learn more about registry types.

**Extract image layer:**
```
GET /v1/images/(image_id)/layer
```

**Insert image layer:**
```
PUT /v1/images/(image_id)/layer
```

**Retrieve an image:**
```
GET /v1/images/(image_id)/json
```
**Retrieve roots of an image:**
```
GET /v1/images/(image_id)/ancestry
```
**Obtain all tags or specific tag of a repository:**
```
GET /v1/repositories/(namespace)/(repository)/tags
```
or
```
GET /v1/repositories/(namespace)/(repository)/tags/(tag*)
```
**Delete a tag:**
```
DELETE /v1/repositories/(namespace)/(repository)/tags/(tag*)
```
**Status check of registry:**
```
GET /v1/_ping
```

### Docker Hub API

Docker Hub API is a simple REST API for Docker Hub. Once again, please refer to part four of this series to learn more about Docker Hub. Docker Hub controls user accounts, authentication by managing checksums, and public namespaces. This API also allows operations pertaining to user and library repositories.

First, let’s explore commands that are specific to library repositories (requires admin access):

1. Create a new repository - A new library repository can be created using the command
```
PUT /v1/repositories/(repo_name)/
```
where repo_name refers to the name of the new repository

2. Delete existing repository - The following command is used to delete an existing repository:
```
DELETE /v1/repositories/(repo_name)/
```
where repo_name indicates the name of the library repository to be deleted.

3. Update repository images - Images that are already in a library repository can be updated using the command
```
PUT /v1/repositories/(repo_name)/images
```
4. Get images from a repository - To fetch an image from any given library, use the command
```
GET /v1/repositories/(repo_name)/images
```
5. Authorization - A library repository is authorized using a token which can be created using the command
```
PUT /v1/repositories/(repo_name)/auth
```
Now, let’s list the commands specific to user repositories. The major difference between library-specific and user-specific commands is the use of namespace.
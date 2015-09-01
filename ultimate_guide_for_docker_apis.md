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
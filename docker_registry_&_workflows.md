# Docker Registry & Workflows

In the previous #Docker Tutorial Series post, we discussed the importance of DockerFile and provided a list of DockerFile commands that makes the automation of image creation easier. In this post, let’s talk about about a significant #Docker component: #Docker Registry. This is the central registry for all repositories, public and private, and their workflows. But, before we dive into #Docker Registry, let’s go over some common terms and concepts related to repositories.

1. Repositories can be “liked” using stars or by being bookmarked.

2. Interact with the community by using the comment service to leave “comments” on the repositories.

3. Private repositories are similar to public ones, except that the former does not show up on search results and give access rights to it. A user, set as a collaborator, only has access to the private repositories.

4. Configure webhooks after a successful push has been made.

### Docker Registry has three roles to play: the index, registry and registry client.

stock_insert_index_markerRole #1 -- Index: The index is responsible for and maintains information about user accounts, checksums of the images, and public namespaces. It maintains such information using the following components:

* Web UI

* Meta-data store

* Authentication service

* Tokenization

It also resolves longer URLs for easier use and authenticates owners to repositories.

**Role #2 -- Registry:** The registry houses the images and graphs for the repositories. However, it does not have a local database and provides no user authentication. Database support is provided by S3, cloud files and local FS. Furthermore, the authentication is taken care of by the Index Auth service using tokens. Registries can be of different types. Let’s analyze a few of them:

1. **Sponsor Registry:** A third-party registry for use by its customers and the #Docker community.

2. **Mirror Registry:** A third-party registry for use by only its customers.

3. **Vendor Registry:** A registry provided by a vendor who distributes #Docker images.

4. **Private Registry:** A registry operated by a private entity with a firewall and additional layers of security.

**clientRole #3** -- Registry Client: #Docker acts as the registry client that maintains push and pull, as well as client authorizations.


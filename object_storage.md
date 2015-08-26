# Object Storage

Object Storage Summary
* Data is stored as objects in one large, scalable pool of storage
* Objects are stored with metadata – information about the object
* An Object ID is stored, to locate the data
* REST is the standard interface, simple commands used by applications
* Objects are immutable; edits are saved as a new object

#### Why Object Storage?

Massive Data Growth
Depending on which analyst firm you talk to, you will hear storage growth predictions that vary between 30x-40x for
the next decade. That means we will all be storing 30 to 40 times as much digital data ten years from now, compared
to today. At the same time, companies will only invest an additional 50% in personnel to manage their storage
infrastructures. This means that the average storage operator will have to manage 15-20 times as much storage a
decade from now. This will drive the need for storage platforms that require little management effort and scale out
to virtually unlimited capacities.

####Always Online
Much of that data growth is driven by the recent innovations in cloud and mobile computing. We already mentioned
Amazon S3, but there are also Google®, Facebook®, Apple® and several smaller public storage cloud offerings that set
a new level of expectations where all data needs to be available anywhere at anytime. 


#### Power to the Applications
File-based storage platforms not only fail to scale suffi ciently, they also become obsolete as more and more
applications are designed to use REST API’s (the default interface for object storage platforms) to talk directly to
the storage, without additional (fi le system) layers in between. This greatly simplifi es architectures and delivers
signifi cant performance gains.

#### The Big Data Explosion
Essentially, there are three types of Big Data: Big Structured Data, Big Semi-structured Data (Big Data Analytics) and Big Unstructured Data. All three require one or more of the “three V’s”, the commonly accepted defi nition of Big Data: “Big Data refers to any set of data that comes in great Volumes, has a large Variety of information and/or is consumed at high Velocity.

Big Structured Data refers to large enterprise databases. Velocity is key here, hence the success of the superfast
SSD drives. Big Semi-structured Data refers to massive volumes of small log fi les (often sensor information), that is collected for analytics. Therefore, we also talk about Big Data Analytics. This data is stored in distributed frameworks
that support distributed processing. Think of Hadoop® and MapReduce. Object Storage is not commonly used for
structured or semi-structured Big Data, unless it is for archival purposes.

The sweet spot for object storage is Big Unstructured Data, which refers to all data that users best understand
as fi les. Think of image and movie data – always growing and always in higher resolution – music fi les, offi ce
documents etc. Analysts believe that 80% or more of the expected data growth will be unstructured data. File based
storage platforms cannot support this growth. This is the problem Object Storage solves.



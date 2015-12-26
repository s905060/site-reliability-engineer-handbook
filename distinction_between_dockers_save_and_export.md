# Distinction between Docker's Save and Export

###Persistence docker way mirror or container

Docker mirror and the container can be derived in two ways

* docker save #ID or #Name
* docker export #ID or #Name

###Difference between the two ways of

For Docker Save method will save all the history of the mirror

#### Export as tar 
`docker Save #ID or #Name> /home/save.tar`

#### Import tar 
`docker load <  / home / Save . tar`

For Docker Export method does not preserve history, that history does not commit

#### Export as tar 
`docker export  #ID or #Name> /home/export.tar`

#### Import tar 
`cat / home / export . tar | docker Import  - some_image_name`
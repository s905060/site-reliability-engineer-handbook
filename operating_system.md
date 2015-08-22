#What is the difference between soft and hard links

###Soft Links => 
1. Soft link files will have different inode numbers then source file

2. If original file deleted then soft link file be of no use

3. Soft links are not updated

4. Can create links between directories

5. Can cross file system boundaries

###Hard Links => 

1. Hard links will have the same inode number as source file

2. Hard links can not link directories

3. Can not cross file system boundaries

4. Hard links always refers to the source, even if moved or removed
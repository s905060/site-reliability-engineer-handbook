# Common hadoop command

Create a directory

`hadoop dfs - mkdir / home`

Upload file or directory to hdfs
```
hadoop dfs - put hello / 
hadoop dfs - put hellodir /  /
```

View catalog

`hadoop dfs - LS /`

Create an empty file

`hadoop dfs - touchz / Wahaha`

Delete a file

`hadoop dfs - RM / Wahaha`

Remove a directory

`hadoop dfs - rmr / home`

Rename

`hadoop dfs - mv / hello1 / hello2`

View Files

`hadoop dfs - cat / hello`

All content will be developed under the directory merge into one file, downloaded to the local

`hadoop dfs - getmerge / hellodir wa`

Use du files and directories size

`hadoop dfs - du /`

The catalog copy to the local

`hadoop dfs - copyToLocal   / home localdir`

Check dfs case

`hadoop dfsadmin - report`

View is running a Java program

`jps`
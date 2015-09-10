# ELK

### Delete Mapping

Allow to delete a mapping (type) along with its data. The REST endpoints are
```
[DELETE] /{index}/{type}

[DELETE] /{index}/{type}/_mapping

[DELETE] /{index}/_mapping/{type}
```
where

```
type * | _all | glob pattern | name1, name2, …
```

```
index * | _all | glob pattern | name1, name2, …
```

Note, most times, it make more sense to reindex the data into a fresh index compared to delete large chunks of it.

```
curl -XDELETE 'http://localhost:9200/_all/clustrix_query'
```
# Skywalking 使用 





##ElasticSearch 相关 

```
PUT /_cluster/settings
{
  "transient": {
    "cluster": {
      "max_shards_per_node":10000
    }
  }
}

POST \*2020060\*/_forcemerge?max_num_segments=1


POST  *,-segment*/_forcemerge?max_num_segments=1

```

**Elasticsearch Cluster Troubleshooting**

**Problem**
When UI is not able to load data, sometimes the elasticsearch cluster may have gone RED.

**Solution**
We investigate the cluster health thru API calls using curl commands and validate that our actions turn the indices and cluster back green on Kibana UI.

Occasionally we lose shards when nodes go down and recover. The nodes are usually assigned a different identifier that's when shards go missing. This issue is expected to disappear if we assigned different names to our datanodes so the shards can find their way home when the datanodes get restarted on a different ID but uses the same distinct name.

To find the updated node IDs. 

*curl -u elastic:0TPjdxkQoREG59BP78Fa -k https://elastic-uat-sgrass-uat-az1-elastic-search-zsyvvq.aks-lb1-sgrass-uat-az1-osdvum.pru.intranet.asia/_cluster/allocation/explain?pretty=true
![image](https://github.com/user-attachments/assets/91c81e62-7e30-4413-83c3-da848c4a4d6b)
To find unassigned shards.

* curl -u elastic:0TPjdxkQoREG59BP78Fa -k https://elastic-uat-sgrass-uat-az1-elastic-search-zsyvvq.aks-lb1-sgrass-uat-az1-osdvum.pru.intranet.asia/_cat/shards?h=index,shard,prirep,state,unassigned.reason?pretty=true | grep UNASSIGNED
For each unassigned shard, we can possibly recover them (provided all nodes are already up and running so data doesn't get overwritten or clash). We can use reroute command to reassign a shard of an index.
![image](https://github.com/user-attachments/assets/246e23f9-25ff-428e-9687-e1c7e03cce61)

1 - index with unassigned shard
2 - unassigned shard of index
3 - node ID (if node names are unique we can use the node name for this operation too)
curl -XPOST -u elastic:0TPjdxkQoREG59BP78Fa -k https://elastic-uat-sgrass-uat-az1-elastic-search-zsyvvq.aks-lb1-sgrass-uat-az1-osdvum.pru.intranet.asia/_cluster/reroute -d '{
"commands" : [
{
"allocate_empty_primary" : 
{
"index" : "search-icij", "shard" : 0,
"node" : "nTxqFWynQj2g5Oud1w2eIw",
"accept_data_loss" : "true"
}
}
]
}' -H 'Content-Type: application/json'

==================
==================

**Problem**
When loading large documents in to ElasticSearch, and fails with the following error
``` Job aborted due to stage failure: Task 84 in stage 20.0 failed 4 times, most recent failure: Lost task 84.3 in stage 20.0 (TID 4425, 10.193.19.208, executor 2): org.apache.spark.util.TaskCompletionListenerException: org.elasticsearch.hadoop.rest.EsHadoopRemoteException: es_rejected_execution_exception: rejected execution of coordinating operation [coordinating_and_primary_bytes=536058997, replica_bytes=0, all_bytes=536058997, coordinating_operation_bytes=3050436, max_coordinating_and_primary_bytes=536870912]
```

**Solution**
You can adjust the number of workers/cores to reduce the number of parallel tasks hitting the cluster
![image](https://github.com/user-attachments/assets/e357fdc2-2b49-44b9-9392-bd248a61a120)
Under Configure, you can reduce the number of workers
![image](https://github.com/user-attachments/assets/e7e622da-75b9-4e95-aba5-c3272ad21faf)
Each worker contains a certain number of cores, in the example above, the worker has 32 cores

The number of parallel tasks running = worker * cores

In general, the number of parallel tasks should be kept to around 30 for large documents



**Reducing the cores**

If further need to reduce, under jobs → task
![image](https://github.com/user-attachments/assets/55dfef9f-a2ec-4afb-bd63-75b1fa940a8b)
Click the edit button under cluster
![image](https://github.com/user-attachments/assets/08cc5523-981d-4c26-9deb-a38631395ecd)
Under Advanced Options → Spark Config, add/edit the spark.executor.cores to some lower number

=====================
=====================
**Problem**

When loading large documents in to ElasticSearch, it might fail halfway with the following error 

```EsHadoopNoNodesLeftException: Connection Error```

**Solution**
This indicates an issue where the task fails to insert to all nodes within a specified amount of time. There are a couple of possible resolutions:

Same resolution as the above - reduce the number of parallel tasks by reducing the number of workers

Adjust the elasticsearch config to increase the http.timeout and reduce the bulk sizes

Under /project/prudential-scripts/etl/etl.conf you can add in settings that increase the timeout before failure, this gives the loading task more time before it encounters an error
![image](https://github.com/user-attachments/assets/0f20c604-9536-4e75-aee7-0fd5d6191070)
```

otherOptions {
    "es.nodes.discovery": "false"
    "es.nodes.wan.only": "true"
    "es.http.timeout": "20m"
}
bulk {
    "entries": 500
    "retryWait": 300
    "sizeInMb": 10
}
```

Setting es.http.timeout to a higher value e.g. 20 minutes or 30 minutes may help solve this issue (try lower values first like 15m, as longer values may stall the job if the primary shards fail)

You can also adjust the bulk settings, e.g. setting entries to a lower number or sizeInMb to a lower number


3. If elastic search fails to load, it might be an issue with the shards in the cluster


```curl -XGET "https://elasticlink.here/_cat/shards?v=true&h=index,prirep,shard,store&s=prirep,store&bytes=gb&pretty" -u elastic:pw```
Running this will get the approximate size of each shard containing the indices, each shard should be 20-50gb in size.

If you notice that the shards are large, you can reindex them into smaller shards. Adjust the shards in the config file (keep replica to 1). *Note the picture below shows 0 for replicas for testing purposes
![image](https://github.com/user-attachments/assets/62822c3c-ce19-4020-9c7f-59d4c7a2885d)
Lastly, if all fails to load the data in the first try i.e. 0 out of xx tasks completed in the Spark Job, you can delete the index and rerun the job


```curl -XDELETE https://elasticlink.here/{index_name} -u elastic:pw```

===============
===============

**Problem**
``` org.elasticsearch.hadoop.rest.EsHadoopRemoteException: unavailable_shards_exception: [prudential-uat-2024-01-24_03-48-06-525-resolver-bvd-individual][1] primary shard is not active Timeout: [1m], request: [BulkShardRequest [[prudential-uat-2024-01-24_03-48-06-525-resolver-bvd-individual][1]] containing [256] requests]
```

**Solution**
The primary shard has failed. Options

Delete the old indexes and rerun the load
Adjust the settings to be lower (lowered parallel tasks/ lowered bulk settings) and monitor

=========
**Problem**
Kibana not working with error message: Elastic is not ready

**Solution**
Restart Kibana through kubernetes.

//to-do 

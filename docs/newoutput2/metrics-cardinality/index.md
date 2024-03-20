---
title: "Metrics Cardinality"
date: "2022-12-03"
---

With Coralogix you can get your metrics cardinality (i.e. the number of elements in a set, or grouping, of metrics as defined by query criteria). Our API returns cardinality statistics by dates**\*** in 4 categories:  
  
1\. **seriesCountByMetricName:** Provides a list of metrics names and their series count.

2\. **seriesCountByLabelName:** Provides a list of label names and their series count.

3\. **seriesCountByLabelValuePair:** Provides a list of label-value pairs and their series count.

4\. **labelValueCountByLabelName:** Provides a list of label names and their distinct value counts.  

**\*** Dates are in UTC.

**  
URL**: Select the [Metrics Cardinality API endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/#metrics-cardinality) associated with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

**Method**: GET

**Authorization**: Use the “Alerts, Rules and Tags API Key” in the authorization header.

**Query parameters**:

- topN: Number of highest cardinality metrics to display in each category.
    - It should be higher than 0.
    
    - **10** by default.

- fromDate: The oldest date that will be included into the statistics response.
    - It can’t be in the future.
    
    - It can’t be after toDate.
    
    - **today** by default.  
        

- toDate: The most recent date that will be included into the statistics response.
    - It can’t be in the future.
    
    - It can’t be before fromDate.
    
    - **today** by default.

_**Defaults will be used if all query parameters are omitted.**_

Example #1: topN, fromDate, and toDate.

```
curl 'https://ng-api-http.coralogixstg.wpengine.com/api/metrics/cardinality?topN=2&fromDate=2022-12-01&toDate=2022-12-02' -H 'authorization: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
```

Output:

```
[{"date":"2022-12-02","data":{"totalSeries":120344,"totalLabelValuePairs":1603533,"seriesCountByMetricName":[{"name":"apiserver_request_duration_seconds_bucket","value":31929},{"name":"etcd_request_duration_seconds_bucket","value":7874}],"seriesCountByLabelName":[{"name":"cluster","value":121290},{"name":"job","value":90650}],"seriesCountByLabelValuePair":[{"name":"cluster=onlineboutique","value":121290},{"name":"job=kubernetes-apiservers","value":72421}],"labelValueCountByLabelName":[{"name":"__name__","value":711},{"name":"le","value":159}]}},{"date":"2022-12-01","data":{"totalSeries":325782,"totalLabelValuePairs":5101308,"seriesCountByMetricName":[{"name":"apiserver_request_duration_seconds_bucket","value":105285},{"name":"etcd_request_duration_seconds_bucket","value":26405}],"seriesCountByLabelName":[{"name":"cluster","value":405643},{"name":"job","value":404865}],"seriesCountByLabelValuePair":[{"name":"cluster=onlineboutique","value":405643},{"name":"job=kubernetes-apiservers","value":239969}],"labelValueCountByLabelName":[{"name":"__name__","value":712},{"name":"name","value":200}]}}]
```

Above results in beautified JSON format:

```
[{
	"date": "2022-12-02",
	"data": {
		"totalSeries": 120344,
		"totalLabelValuePairs": 1603533,
		"seriesCountByMetricName": [{
			"name": "apiserver_request_duration_seconds_bucket",
			"value": 31929
		}, {
			"name": "etcd_request_duration_seconds_bucket",
			"value": 7874
		}],
		"seriesCountByLabelName": [{
			"name": "cluster",
			"value": 121290
		}, {
			"name": "job",
			"value": 90650
		}],
		"seriesCountByLabelValuePair": [{
			"name": "cluster=onlineboutique",
			"value": 121290
		}, {
			"name": "job=kubernetes-apiservers",
			"value": 72421
		}],
		"labelValueCountByLabelName": [{
			"name": "__name__",
			"value": 711
		}, {
			"name": "le",
			"value": 159
		}]
	}
}, {
	"date": "2022-12-01",
	"data": {
		"totalSeries": 325782,
		"totalLabelValuePairs": 5101308,
		"seriesCountByMetricName": [{
			"name": "apiserver_request_duration_seconds_bucket",
			"value": 105285
		}, {
			"name": "etcd_request_duration_seconds_bucket",
			"value": 26405
		}],
		"seriesCountByLabelName": [{
			"name": "cluster",
			"value": 405643
		}, {
			"name": "job",
			"value": 404865
		}],
		"seriesCountByLabelValuePair": [{
			"name": "cluster=onlineboutique",
			"value": 405643
		}, {
			"name": "job=kubernetes-apiservers",
			"value": 239969
		}],
		"labelValueCountByLabelName": [{
			"name": "__name__",
			"value": 712
		}, {
			"name": "name",
			"value": 200
		}]
	}
}]
```

Example #2: all query parameters omitted.

```
curl 'https://ng-api-http.coralogixstg.wpengine.com/api/metrics/cardinality?topN=&fromDate=&toDate=' -H 'authorization: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
```

Output:

```
[{"date":"2022-12-02","data":{"totalSeries":121067,"totalLabelValuePairs":1612878,"seriesCountByMetricName":[{"name":"apiserver_request_duration_seconds_bucket","value":32119},{"name":"etcd_request_duration_seconds_bucket","value":7912},{"name":"apiserver_response_sizes_bucket","value":5365},{"name":"rest_client_request_duration_seconds_bucket","value":4943},{"name":"apiserver_storage_list_duration_seconds_bucket","value":2758},{"name":"apiserver_watch_events_sizes_bucket","value":2144},{"name":"storage_operation_duration_seconds_bucket","value":2134},{"name":"kubelet_runtime_operations_duration_seconds_bucket","value":1842},{"name":"container_tasks_state","value":1449},{"name":"container_blkio_device_usage_total","value":911}],"seriesCountByLabelName":[{"name":"cluster","value":122013},{"name":"job","value":121984},{"name":"instance","value":121924},{"name":"__name__","value":121067},{"name":"le","value":71430},{"name":"verb","value":47811},{"name":"component","value":46996},{"name":"version","value":44351},{"name":"resource","value":42355},{"name":"scope","value":42311}],"seriesCountByLabelValuePair":[{"name":"cluster=onlineboutique","value":122013},{"name":"job=kubernetes-apiservers","value":72862},{"name":"instance=172.31.6.22:443","value":42526},{"name":"component=apiserver","value":41181},{"name":"version=v1","value":37223},{"name":"beta_kubernetes_io_arch=amd64","value":28538},{"name":"eks_amazonaws_com_capacityType=ON_DEMAND","value":24434},{"name":"beta_kubernetes_io_os=linux","value":24410},{"name":"eks_amazonaws_com_nodegroup=chen-eks2-nodes","value":24306},{"name":"beta_kubernetes_io_instance_type=t3.large","value":20464}],"labelValueCountByLabelName":[{"name":"__name__","value":711},{"name":"le","value":159},{"name":"type","value":147},{"name":"name","value":146},{"name":"resource","value":130},{"name":"id","value":116},{"name":"device","value":87},{"name":"url","value":79},{"name":"replicaset","value":77},{"name":"kind","value":57}]}}]
```

Above results in beautified JSON format:

```
[{
	"date": "2022-12-02",
	"data": {
		"totalSeries": 121067,
		"totalLabelValuePairs": 1612878,
		"seriesCountByMetricName": [{
			"name": "apiserver_request_duration_seconds_bucket",
			"value": 32119
		}, {
			"name": "etcd_request_duration_seconds_bucket",
			"value": 7912
		}, {
			"name": "apiserver_response_sizes_bucket",
			"value": 5365
		}, {
			"name": "rest_client_request_duration_seconds_bucket",
			"value": 4943
		}, {
			"name": "apiserver_storage_list_duration_seconds_bucket",
			"value": 2758
		}, {
			"name": "apiserver_watch_events_sizes_bucket",
			"value": 2144
		}, {
			"name": "storage_operation_duration_seconds_bucket",
			"value": 2134
		}, {
			"name": "kubelet_runtime_operations_duration_seconds_bucket",
			"value": 1842
		}, {
			"name": "container_tasks_state",
			"value": 1449
		}, {
			"name": "container_blkio_device_usage_total",
			"value": 911
		}],
		"seriesCountByLabelName": [{
			"name": "cluster",
			"value": 122013
		}, {
			"name": "job",
			"value": 121984
		}, {
			"name": "instance",
			"value": 121924
		}, {
			"name": "__name__",
			"value": 121067
		}, {
			"name": "le",
			"value": 71430
		}, {
			"name": "verb",
			"value": 47811
		}, {
			"name": "component",
			"value": 46996
		}, {
			"name": "version",
			"value": 44351
		}, {
			"name": "resource",
			"value": 42355
		}, {
			"name": "scope",
			"value": 42311
		}],
		"seriesCountByLabelValuePair": [{
			"name": "cluster=onlineboutique",
			"value": 122013
		}, {
			"name": "job=kubernetes-apiservers",
			"value": 72862
		}, {
			"name": "instance=172.31.6.22:443",
			"value": 42526
		}, {
			"name": "component=apiserver",
			"value": 41181
		}, {
			"name": "version=v1",
			"value": 37223
		}, {
			"name": "beta_kubernetes_io_arch=amd64",
			"value": 28538
		}, {
			"name": "eks_amazonaws_com_capacityType=ON_DEMAND",
			"value": 24434
		}, {
			"name": "beta_kubernetes_io_os=linux",
			"value": 24410
		}, {
			"name": "eks_amazonaws_com_nodegroup=chen-eks2-nodes",
			"value": 24306
		}, {
			"name": "beta_kubernetes_io_instance_type=t3.large",
			"value": 20464
		}],
		"labelValueCountByLabelName": [{
			"name": "__name__",
			"value": 711
		}, {
			"name": "le",
			"value": 159
		}, {
			"name": "type",
			"value": 147
		}, {
			"name": "name",
			"value": 146
		}, {
			"name": "resource",
			"value": 130
		}, {
			"name": "id",
			"value": 116
		}, {
			"name": "device",
			"value": 87
		}, {
			"name": "url",
			"value": 79
		}, {
			"name": "replicaset",
			"value": 77
		}, {
			"name": "kind",
			"value": 57
		}]
	}
}]
```

  
**Still have questions?** If so, please reach out to us via our in-app chat for quick assistance.

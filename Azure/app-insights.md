## Query

`traces | union exceptions | sort by timestamp`


#### APIM

Plot usage of each operation over time  
```
requests
| where timestamp > ago(7d)
| where operation_Name startswith "ApiName"
| summarize count() by bin(timestamp, 1h), operation_Name
| render timechart
```

List response time stats per operation
```
requests
| where timestamp > ago(7d)
| where operation_Name startswith "ApiName"
| summarize avg(duration), min(duration), max(duration), percentile(duration, 95) by operation_Name
```

Also includes variations of query parameters to determine benefits of caching
```
requests
| where timestamp > ago(7d)
| where operation_Name startswith "ApiName"
| summarize avg_duration=avg(duration), min_duration=min(duration), max_duration=max(duration), percentile_duration_95=percentile(duration, 95), unique_query_variations=dcount(url) by operation_Name
```

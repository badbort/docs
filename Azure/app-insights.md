## Query

`traces | union exceptions | sort by timestamp`


#### APIM

```
requests
| where timestamp > ago(7d)
| where operation_Name contains "ApiName"
| summarize count() by bin(timestamp, 1h), operation_Name
| render timechart
```

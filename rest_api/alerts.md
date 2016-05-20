# Alerts

## Get list of alerts

**URL**: GET /api/alerts

**Request parameters**: None

**Response parameters**

* `alerts`: List of [alert](alerts.md#get-alert) items


## Get alert

**URL**: GET /api/alert/:id

**Request parameters**: None

**Response parameters**

* `id`: 
* `type`: 
* `name`: 
* `enabled`: 
* `filter`: 
* `condition`: 
* `segmentBy`: 
* `segmentCondition`: 
* `timespan`: 
* `severity`: 
* `notify`: 
* `version`: 
* `createdOn`: 
* `modifiedOn`: 
* `notificationCount`: 

**Example**

```
GET /api/alerts/123

{
    "alert": {
        "id":               123,
        "version":          7,
        "createdOn":        1459198751000,
        "modifiedOn":       1460994864000,
        "type":             "MANUAL",
        "name":             "My Alert",
        "enabled":           true,
        "filter":            "cloudProvider.tag.Name = \"clients\"",
        "severity":          7,
        "notify":            [ "EMAIL" ],
        "timespan":          60000000,
        "notificationCount": 1,
        "segmentBy":         [ "agent.tag.infrastructure" ],
        "segmentCondition":  {
            "type": "ANY"
        },
        "condition":         "max(sum(memory.used.percent)) >= 1"
    }
}
```

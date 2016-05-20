# Alerts

## Get list of alerts

**URL**: GET /api/alerts

**Request parameters**: None

**Response**
```
{
    "alerts": (list of alert)
}
```

* `alerts`: List of [alert]() items.


## Get alert

**URL**: GET /api/alert/:id

**Request parameters**: None

**Response**
```
{
    "alert": {
        "id":                (number),
        "version":           (number),
        "createdOn":         (timestamp),
        "modifiedOn":        (timestamp),
        "type":              (type ID),
        "name":              (string),
        "enabled":           (boolean),
        "filter":            (string),
        "condition":         (string),
        "segmentBy":         (array of string),
        "segmentCondition":  {
                                 "type": (segment type ID)
                             },
        "timespan":          (timespan),
        "severity":          (number),
        "notify":            (array),
        "notificationCount": (number)
    }
}
```

* `id`: 
* `version`: 
* `createdOn`: 
* `modifiedOn`: 
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
* `notificationCount`: 

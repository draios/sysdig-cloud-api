# Alerts

**Note** : You can use our Python client to create, list, delete, update and restore Alerts https://github.com/draios/python-sdc-client/tree/master/examples

## Get list of alerts

**URL**: `GET /api/alerts`

**Request parameters**: None

**Response parameters**

* `alerts`: List of [alert](alerts.md#get-alert) items


## Get alert

**URL**: `GET /api/alert/:id`

**Request parameters**: None

**Response parameters**

* `id`: Alert ID
* `type`: Type of alert; Valid values are:
  * `MANUAL` for manual alerts
  * `BASELINE` for baseline alerts
  * `HOST_COMPARISON` for host comparison alerts
* `name`: Name of the alert; Note that alert names must be unique
* `enabled`: `true` if the alert is being processed and events can fire; `false` otherwise
* `filter`: String-encoded filter of the alert; The filter can be used to select nodes and/or entities
* `condition`: Valid for manual alerts only; Configures the threshold for the alert
* `segmentBy`: Segmentation to apply to condition, if needed
* `segmentCondition`: If `segmentBy` is set, it configures whether alert events will be triggered when *all* segments reach the threshold or *at least one* does. The format is an object with a `type` property with `ALL` or `ANY` respectively (e.g. `{ "type": "ANY" }`
* `timespan`: Number of microseconds; Minimum time interval for which the alert condition must be met before the alert will fire a event; Minimum value is 60000000 (1 minute) and values must be multiple of 60000000 (1 minute)
* `severity`: `null` to instruct the alert to set event severity automatically, a number from 0 (_emergency_) to 7 (_debug_) to set a manual severity
* `notificationChannelIds`: List of notification channel identifiers; 

**Note**: Notifications must be configured and enabled globally in the Settings > Notification page of Sysdig Cloud
* `version`: Revision version of the alert configuration
* `createdOn`: Unix-timestamp of time when the alert was created
* `modifiedOn`: Unix-timestamp of time when the alert was last modified
* `notificationCount`: Number of events fired for the alert during the past 2 weeks

**Errors**

* `404 Not Found` if the alert ID is not found
* `400 NotificationChannelId: {id} does not exist` if the notification channel id specified in the `notificationChannelIds` property does not exist

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
        "timespan":          60000000,
        "notificationCount": 1,
        "segmentBy":         [ "agent.tag.infrastructure" ],
        "segmentCondition":  {
            "type": "ANY"
        },
        "condition":         "max(sum(memory.used.percent)) >= 1",
        "sysdigCapture" : {
                            "enabled": true,
                            "name": "testName",
                            "filters": "test",
                            "duration": 104857600,
                            "type": "LOCAL",
                            "bucketName":  "bucketName",
                            "folder" : "folder"
                        }
    }
}
```


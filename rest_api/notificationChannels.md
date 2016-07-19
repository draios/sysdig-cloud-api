# Notification Channels

**Table of Contents**

<!-- toc -->

## Get list of Notification Channels

**URL**: `GET /api/notificationChannels`

**Request url parameters**: None

**Request body parameters**: None

**Response body parameters**

* `notificationChannels`: List of [notificationChannels](notificationChannels.md#get-notification-channel)

**Example**

```
GET /api/notificationChannels/

{
  "notificationChannels": [
    {
      "id": 20,
      "version": 1,
      "createdOn": 1466023669000,
      "modifiedOn": 1466023669000,
      "type": "EMAIL",
      "enabled": true,
      "options": {
        "emailRecipients": [
          "test@sysdig.com"
        ],
        "notifyOnOk": false
      }
    }
  ]
}
```

## Get notification channel

**URL**: `GET /api/notificationChannels/:id`

**Request url parameters**: `id`

**Request body parameters**: None

**Response body parameters**:

* `id`: NotificationChannel ID
* `type`: Type of notification channels; Valid values are:
  * `EMAIL` for email notifications
  * `SNS` for aws sns notifications
  * `PAGER_DUTY` for pager duty notifications
  * `SLACK` for slack notifications
  * `VICTOROPS` for victorOps notifications
* `name`: Optional name of the notification channel; Note that notification channel names must be unique and no more than 255 characters
* `enabled`: `true` if the notification channel is being processed and events can fire; `false` otherwise
* `options`: this contains different properties related to the different notification channel type:
        * `EMAIL`
        ** `emailRecipients` is a list of email addresses
        * `SNS`
        **    `snsTopicARNs` is a list of AWS SNS arn topics
        * `SLACK`
        **    `channel` slack channel name
        **    `notifyOnOk` boolean flag to receive a notification message when the notification state changed from ACTIVE to OK
        **    `url`  slack incoming webhook url endpoint (https://api.slack.com/incoming-webhooks)
        * `PAGER_DUTY`
        **    `channel` pagerDuty channel name
        **    `resolveOnOk` boolean flag to send a notification to resolve the incident in PD when the notification state changed from ACTIVE to OK
        * `VICTOROPS`
        **    `apiKey` mandatory api key retrieved from VictorOps integration settings page
        **    `routingKey` mandatory routing key retrieved from VictorOps integration settings page 
        **    `resolveOnOk` boolean flag to send a notification to resolve the incident in VictorOps when the notification state changed from ACTIVE to OK
 
**Note**: The notification channels can be enabled by the alert
 
* `version`: Revision version of the notification channel configuration
* `createdOn`: Unix-timestamp of time when the notification channel was created
* `modifiedOn`: Unix-timestamp of time when the notification channel was last modified

**Errors**

* `404 Not Found` if the notification channel ID is not found

**Example by Type**

Type: EMAIL

```
{
  "notificationChannel": {
    "id": 20,
    "version": 1,
    "createdOn": 1466023669000,
    "modifiedOn": 1466023669000,
    "type": "EMAIL",
    "enabled": true,
    "name": "emailChannel",
    "options": {
      "emailRecipients": [
        "sergio@sysdig.com"
      ],
      "notifyOnOk": false
    }
  }
}
```

Type: SNS

```
{
  "notificationChannel": {
    "id": 23,
    "version": 1,
    "createdOn": 1466023755000,
    "modifiedOn": 1466023755000,
    "type": "SNS",
    "enabled": true,
    "name": "snsChannel",
    "options": {
      "snsTopicARNs": [
        "arn:aws:sns:us-east-1:273107874544:sergiotest2"
      ],
      "notifyOnOk": false
    }
  }
}
```

Type: SLACK

```
{
  "notificationChannel": {
    "id": 31,
    "version": 2,
    "createdOn": 1466095456000,
    "modifiedOn": 1466095473000,
    "type": "SLACK",
    "enabled": true,
    "name": "slackChannel",
    "options": {
      "notifyOnOk": false,
      "channel": "slackin"
      "url" : "https://hooks.slack.com/services/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/"
    }
  }
}
```

Type: PAGER_DUTY

```
{
  "notificationChannel": {
    "id": 35,
    "version": 1,
    "createdOn": 1466449404000,
    "modifiedOn": 1466449404000,
    "type": "PAGER_DUTY",
    "enabled": true,
    "name": "pdChannel",
    "options": {
      "account": "draios-test",
      "serviceKey": "6ce6c022c87a8843aa2f97c86be06c81",
      "serviceName": "Sergio-test",
      "resolveOnOk": true
    }
  }
}
```


Type: VICTOROPS

```
{
  "notificationChannel": {
    "id": 36,
    "version": 1,
    "createdOn": 1466456928000,
    "modifiedOn": 1466456928000,
    "type": "VICTOROPS",
    "enabled": true,
    "name": "victorOpsChannel",
    "options": {
      "resolveOnOk": true,
      "apiKey": "01ce050e-90c1-4ba6-aa59-f5ae5849557e",
      "routingKey": "myfaketeam"
    }
  }
}

```

## Create notification channel

**URL**: `POST /api/notificationChannels/`

**Request url parameters**: None

**Request body parameters**: All the response body parameters specified in Get notification channel except:
  * `id`
  * `version`
  * `createdOn`
  * `modifiedOn`

**Response parameters**: See Get Notification Channels

**Errors**

* `404 Not Found` notification channel ID not found
* `422 Name length must be between 1 and 255 characters` wrong length name
* `422 The parameter notificationChannel.type is set to an invalid value. Valid values are: EMAIL|PAGER_DUTY|SLACK|SNS|VICTOROPS` not allowed notificationChannel.type
* `422 The url endpoint is not valid. It should start with: 'https://hooks.slack.com/services/` not allowed url endpoint
* `422 The apiKey is missing inside options` apiKey is mandatory for victorOps channel
* `422 The routingKey is missing inside options` routingKey is mandatory for victorOps channel

## Modify notification channel

**URL**: `PUT /api/notificationChannels/:id`

**Request url parameters**: `id`

**Request body parameters**: All the response body parameters specified in Get notification channel except:
  * `createdOn`
  * `modifiedOn`
  
**Response parameters**: See the Get notification Channels 

**Note**
It is not possible to modify the slack channel name

**Errors**

* `404 Not Found` notification channel ID not found
* `409 Conflict version` wrong version number
* `422 Name length must be between 1 and 255 characters` wrong length name
* `422 The parameter notificationChannel.type is set to an invalid value. Valid values are: EMAIL|PAGER_DUTY|SLACK|SNS|VICTOROPS` not allowed notificationChannel.type
* `422 The url endpoint is not valid. It should start with: 'https://hooks.slack.com/services/` not allowed url endpoint
* `422 The apiKey is missing inside options` apiKey is mandatory for victorOps channel
* `422 The routingKey is missing inside options` routingKey is mandatory for victorOps channel
* `422 Missing notification channel id` missing notification channel id

## Delete notification channel

**URL**: `DELETE /api/notificationChannels/:id`

**Request url parameters**: `id`

**Request body parameters**: None

**Response parameters**: None
* `204 No content`

**Errors**:
* `404 Not Found` notification channel ID not found

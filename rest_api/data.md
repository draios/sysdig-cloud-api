# Data API

## Endpoint

Data can be extracted through the following endpoint: `POST /api/data`

Here's a complete example of the HTTP request to fetch a time series:

```
POST /api/data
Authorization: Bearer TOKEN_HERE
Content-Type: application/json
X-Sysdig-Product: SDC

{
  "last": 600,
  "sampling": 10,
  "metrics": [
    {
      "id": "cpu.used.percent",
      "aggregations": {
        "time": "timeAvg",
        "group": "avg"
      }
    }
  ],
  "dataSourceType": "host",
  "filter": null
}
```

The following sections will show different use cases and the related HTTP request bodies.


## Time window
1. Any timestamp is expressed in seconds (Unix timestamp)
2. `start` and `end` not required if `last` is specified
3. `sampling` is required for sampled data requests only
4. `last` not required if `start` and `end` field are specified
4. `sampling` accepts one of the following values accordingly to the customer plan settings:
   * 10 (10 seconds) or 1 (1 seconds)
   * 60 (1 minute)
   * 600 (10 minutes)
   * 3600 (1 hour)
   * 86400 (1 day)
5. If `sampling` is not specified (i.e. aggregated data request):
   1. starting from the highest sampling frequency (1 or 10), the backend finds the first timeline suitable for the specified time window and if it does not exist it will return an error
   2. the time window is aligned according to the sampling time available
   3. **example**
      * Given the request `start: 18:47:51` and `end: 18:49:31` 
      * Adjusted time window is rounded to sampling 10-sec  `start: 18:47:50` and  `end: 18:49:30` 
6. If `sampling` is specified (i.e. sampled data request):
   1. if `sampling` is not one of the available sampling, the backend returns error
   2. if `(end - start) / sampling` is over the the maximum number of samples `(600)` the backend returns error
   3. if `sampling` is valid the time window is adjusted according to the `sampling`
   4. **example**
      * Given the request `start: 4:05:15` (1457323515), `end: 20:16:10` (1457381770), and `sampling: 3600`
      * Adjusted time window is rounded to sampling: `start: 4:00:00` and `end: 20:00:00` 
7. if `last` is specified (i.e. last available seconds):
   1. if `last / sampling` is over the maximum number of samples (`600`) the backend returns error 
   2. if the `sampling` is not specified the backend tries to set `start` and `end` accordingly the timeline that could satisfy the maximum sample number constraint.


## Use cases
The following use cases are based on the panels you can use in [Sysdig Cloud](https://app.sysdigcloud.com).

### Single-line time series
```
{
    start:          1,
    end:            10,
    sampling:       1,
    metrics:        [
                        { id: 'cpu.used.percent', aggregations: { time:'sum', group:'sum' } }
                    ],
    dataSourceType: 'container',
    filter:         'host.hostName = ip-1-2-3-4'
}
```

```
{
    start       1,
    end:        10,
    sampling:   1,
    data:       [
                    { t: 1, d: [ [ 1.5 ] ] },
                    { t: 2, d: [ [ 2.0 ] ] },
                    ...
                    { t: 9, d: [ [ 0.7 ] ] },
                ]
}
```

**Notes**

1. The `filter` is optional
2. `dataSourceType` is optional, can be either `container` or `host`, default value `host`
3. `aggregations` for metrics is optional and it can not be specified for `string` metrics

### Multi-line time series
```
{
    start:          1,
    end:            10,
    sampling:       1,
    metrics:        [
                        { id: 'proc.name' }
                        { id: 'cpu.used.percent', aggregations: { time:'sum', group:'sum' } }
                    ],
    dataSourceType: 'container'
}
```
```
{
    start:      1,
    end:        10,
    sampling:   1,
    data:       [
                    { t: 1, d: [ [ 'proc1', 1.5 ], [ 'proc2', 1.3 ], [ 'proc3', 3.5 ] ] },
                    { t: 2, d: [ [ 'proc1', 2.0 ], [ 'proc2', 1.3 ], [ 'proc3', 3.5 ] ] },
                    ...
                    { t: 9, d: [ [ 'proc1', 0.7 ], [ 'proc2', 1.3 ], [ 'proc3', 3.5 ] ] },
                ]
}
```

### Single-bar chart
```
{
    start:          1,
    end:            10,
    metrics:        [
                        { id: 'proc.name' }
                        { id: 'cpu.used.percent', aggregations: { time:'sum', group:'sum' } }
                    ],
    dataSourceType: 'container'
}
```
```
{
    start:      1,
    end:        10,
    data:       [
                    { t: 1, d: [ [ 'proc1', 1.5 ], [ 'proc2', 1.3 ], [ 'proc3', 3.5 ] ] }
                ]
}
```

### Multi-value bar chart
```
{
    start:          1,
    end:            10,
    metrics:        [
                        { id: 'cpu.used.percent',       aggregations: { time: 'sum', group: 'sum' } }
                        { id: 'memory.used.percent',    aggregations: { time: 'sum', group: 'sum' } }
                        { id: 'fs.used.percent',        aggregations: { time: 'sum', group: 'sum' } }
                    ],
    dataSourceType: 'container'
}
```
```
{
    start:      1,
    end:        10,
    data:       [
                    { t: 1, d: [ [ 1.5, 1.3, 3.5 ] ] }
                ]
}
```

### Single-number
```
{
    start:          1,
    end:            10,
    metrics:        [
                        { id: 'cpu.used.percent', aggregations: { time:'sum', group:'sum' } }
                    ],
    dataSourceType: 'container'
}
```
```
{
    start:      1,
    end:        10,
    data:       [
                    { t: 1, d: [ [ 1.5 ] ] }
                ]
}
```

### Table
```
{
    start:          1,
    end:            10,
    metrics:        [
                        { id: 'host.hostName' }
                        { id: 'host.mac' }
                        { id: 'agent.tag.role' }
                        { id: 'cpu.used.percent',       aggregations: { time: 'sum', group: 'sum' } }
                        { id: 'memory.used.percent',    aggregations: { time: 'sum', group: 'sum' } }
                        { id: 'fs.used.percent',        aggregations: { time: 'sum', group: 'sum' } }
                    ],
    dataSourceType: 'container'
}
```
```
{
    start:      1,
    end:        10,
    data:       [
                    {
                        t: 1,
                        d: [
                            [ 'ip-1-2-3-4', '00:11:22:33:44:55', 'web', 1.2, 2.3, 3.4 ],
                            [ 'ip-1-2-3-5', '00:11:22:33:44:66', 'web', 1.3, 2.4, 3.5 ],
                            [ 'ip-1-2-3-6', '00:11:22:33:44:77', 'web', 1.4, 2.5, 3.6 ],
                        ]
                    }
                ]
}
```

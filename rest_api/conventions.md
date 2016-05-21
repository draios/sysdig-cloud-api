# Conventions

## Resources

The REST API allows you to do two things:

1. Handle resources
2. Execute operations

A resource can be a piece of configuration, a user, a dashboard, an alert, and so on.

Here is a list of conventions used by the REST API for resources:

1. List of resources: The URL uses the plural name for the resource, e.g:
    ```
    GET /api/alerts
    
    {
        "alerts": [ ... ]
    }
    ```

2. Creation of a resource: The URL uses the plural name and the request envelop uses the singular name, e.g.:
   ```
   POST /api/alerts
   {
       "alert": { ... }
   }
   ```
3. Get one resource: The URL uses the plural name, and the response envelop uses the singular name, e.g.:
    ```
    GET /api/alerts/123
    
    {
        "alert": { ... }
    }
    ```

## Encoding

The request should set the HTTP header

```
Accept: application/json
```

while every response is returned with the HTTP header

```
Content-Type: application/json;charset=UTF-8
```

In order to reduce the size of the request and mainly the response, you can set the header

```
Accept-Encoding:gzip, deflate, sdch
```

to compress HTTP body and response.
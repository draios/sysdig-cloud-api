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

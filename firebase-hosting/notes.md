# Firebase Hosting: Notes

# Redirects

```json
"hosting": {
  // Add the "redirects" section within "hosting"
  "redirects": [ {
    "source" : "/foo",
    "destination" : "/bar",
    "type" : 301
  }, {
    "source" : "/firebase/*",
    "destination" : "https://firebase.google.com",
    "type" : 302
  } ]
}
```

# Rewrites

```json
"hosting": {
  // Add the "rewrites" section within "hosting"
  "rewrites": [ {
    "source": "**",
    "destination": "/index.html"
  } ]
}
```

# Headers

```json
"hosting": {
    // Add the "headers" section within "hosting".
    "headers": [ {
      "source" : "**/*.@(eot|otf|ttf|ttc|woff|font.css)",
      "headers" : [ {
        "key" : "Access-Control-Allow-Origin",
        "value" : "*"
    } ]
    }, {
      "source" : "**/*.@(jpg|jpeg|gif|png)",
      "headers" : [ {
      "key" : "Cache-Control",
      "value" : "max-age=7200"
      } ]
    }, {
      // Sets the cache header for 404 pages to cache for 5 minutes
      "source" : "404.html",
      "headers" : [ {
      "key" : "Cache-Control",
      "value" : "max-age=300"
      } ]
    } ]
 }
```

# Connect a Cloud Function

```json
{
  "hosting": {
    "public": "public",

    // Add the following rewrites section *within* "hosting"
   "rewrites": [ {
      "source": "/bigben", "function": "bigben"
    } ]

  }
}
```

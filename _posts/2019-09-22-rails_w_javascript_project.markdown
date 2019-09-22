---
layout: post
title:      "Rails w/ Javascript Project - CORS issue"
date:       2019-09-22 15:21:47 -0400
permalink:  rails_w_javascript_project
---


For our recent project we used our previously built Rails app as our backend API to include new Javascript features. It was very cool to be able to update various actions to occur without necessitating a page reload, and it definitely gave the app a feel closer to how current, modern web apps behave. 

One of the first requirements was to render a resource index page with Javascript and an Active Model Serialization JSON Backend, and one of the most confusing hurdles I encountered while trying to set this up once I generated my reource serializers was the following error:

```
Access to fetch at 'http://localhost:3000/contributions' from origin 'http://0.0.0.0:3000' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
```

According to this documentation (https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS), "For security reasons, browsers restrict cross-origin HTTP requests initiated from scripts." So despite the fact that I was utilizing this very rails app as my backend API, it was still being treated as having a different, external origin when I was trying to pull my data with an AJAX request, or even pass data with a fetch post request. 

As a solution, I had to install this gem (https://github.com/cyu/rack-cors) and follow the Rails configuration instructions provided to give my app the middleware necessary to be able to handle cross domain AJAX calls. This helped to provide the headers necessary to allow my fetch requests access despite any origin. It appeared that using Rails 5 was the reason that I needed to rely on this middleware in order to do so. 





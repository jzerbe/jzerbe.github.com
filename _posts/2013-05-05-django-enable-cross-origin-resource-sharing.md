---
layout: post
title: Django enable Cross Origin Resource Sharing
tags:
- Django
- CORS
published: true
---
I am using a [django-nonrel](https://github.com/django-nonrel/django)
setup, and I needed to enable _Cross-Origin Resource Sharing_.

Relevant reading:

- [W3C spec](http://www.w3.org/TR/cors/) is informative but not immediately helpful
- the [enable CORS cheat-sheet](http://enable-cors.org/) has a fast-action listing of relevant headers
- build [CORS in vanilla JavaScript](http://www.nczonline.net/blog/2010/05/25/cross-domain-ajax-with-cross-origin-resource-sharing/)
- build [AJAX in vanilla JavaScript](http://www.xul.fr/en-xml-ajax.html)


### add items to HttpResponse dictionary

From <https://docs.djangoproject.com/en/dev/ref/request-response/#setting-headers>:
>To set or remove a header in your response, treat it like a dictionary:
> 
>       response = HttpResponse()
>       response['Cache-Control'] = 'no-cache'
>       del response['Cache-Control']
> 


### function pastie

    def addCORSHeaders(theHttpResponse):
        if theHttpResponse and isinstance(theHttpResponse, HttpResponse):
            theHttpResponse['Access-Control-Allow-Origin'] = '*'
            theHttpResponse['Access-Control-Max-Age'] = '120'
            theHttpResponse['Access-Control-Allow-Credentials'] = 'true'
            theHttpResponse['Access-Control-Allow-Methods'] = 'HEAD, GET, OPTIONS, POST, DELETE'
            theHttpResponse['Access-Control-Allow-Headers'] = 'origin, content-type, accept, x-requested-with'
        return theHttpResponse

From <https://github.com/Vraid-Systems/storagebin/blob/master/storagebin/__init__.py> 

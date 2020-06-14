---
layout: post
title: html5 FormData multipart upload to Django backend
tags:
- html5
- FormData
- multipart
- JavaScript
- Django
published: true
---
### FormData

As of Chrome 7, Firefox 4, IE 10, Opera 12, and Safari 5 - it is quite possible
to programmatically upload file data.
[https://developer.mozilla.org/en-US/docs/DOM/XMLHttpRequest/FormData#Browser_compatibility](https://developer.mozilla.org/en-US/docs/DOM/XMLHttpRequest/FormData#Browser_compatibility)

From [SBObj.js](https://github.com/Vraid-Systems/storagebin_js/blob/master/SBObj.js):

    this.PUT = function(theData, theDataContentType) {
        theDataContentType = this.getDefault(theDataContentType, "text/plain");
        
        var aBlob = new Blob([ theData ], {
            type: theDataContentType
        });
        
        var aFormData = new FormData();
        aFormData.append("file", aBlob);
        
        var aDataObj = {
            "type": false,
            "content": aFormData
        };
        
        private_SendDataObj(private_METHOD_POST, aDataObj);
    };

Where `private_SendDataObj` is essentially a wrapper for `xhr.send(aDataObj.content);`. Be sure that there are __NO__
`setRequestHeader` method calls on the `xhr` object; even though the
[_Django: File Uploads_](https://docs.djangoproject.com/en/dev/topics/http/file-uploads/)
documentation tells you otherwise.


### Django\'s request.FILES

[Google App Engine](https://developers.google.com/appengine/docs/python/overview) with
[django-nonrel](http://django-nonrel.org/) to be specific, we can pull in the file data.

From
[storagebin/__init__.py](https://github.com/Vraid-Systems/storagebin/blob/master/storagebin/__init__.py)
and
[storagebin/internal/blobstore.py](https://github.com/Vraid-Systems/storagebin/blob/master/storagebin/internal/blobstore.py):

    elif request.method == 'POST':
        if request.FILES and len(request.FILES) == 1:
            uploaded_file = request.FILES['file']
            
            ...
            
            # write new data
            file_name = files.blobstore.create(uploaded_file.content_type)
            with files.open(file_name, 'a') as f:
                f.write(uploaded_file.read())
            files.finalize(file_name)

refs:
[https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.HttpRequest.FILES](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.HttpRequest.FILES),
[https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.UploadedFile](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.UploadedFile),
[https://developers.google.com/appengine/docs/python/blobstore/overview#Writing_Files_to_the_Blobstore](https://developers.google.com/appengine/docs/python/blobstore/overview#Writing_Files_to_the_Blobstore)

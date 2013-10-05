---
layout: post
title: Django enable Cross Origin Resource Sharing
tags:
- Django
- CORS
published: true
---
I am using a <a href="https://github.com/django-nonrel/django">django-nonrel</a>
setup, and I needed to enable
<abbr title="Cross-Origin Resource Sharing">CORS</abbr>. Relevant reading:
<ul>
    <li><a href="http://www.w3.org/TR/cors/">W3C spec</a> is informative but not immediately helpful</li>
    <li>the <a href="http://enable-cors.org/">enable CORS cheat-sheet</a> has a fast-action listing of relevant headers</li>
    <li>build <a href="http://www.nczonline.net/blog/2010/05/25/cross-domain-ajax-with-cross-origin-resource-sharing/">
        CORS in vanilla JavaScript
    </a></li>
    <li>build <a href="http://www.xul.fr/en-xml-ajax.html">AJAX in vanilla JavaScript</a></li>
</ul>
<br />
<strong>add items to HttpResponse dictionary</strong><br />
From the <a href="https://docs.djangoproject.com/en/dev/ref/request-response/#setting-headers">django manual</a>:
<blockquote>To set or remove a header in your response, treat it like a dictionary:<br />
    <code>
response = HttpResponse()<br />
response['Cache-Control'] = 'no-cache'<br />
del response['Cache-Control']<br />
    </code>
</blockquote>
<br />
<strong>function pastie</strong><br />
<blockquote>
    <code>
def addCORSHeaders(theHttpResponse):<br />
&nbsp;&nbsp;&nbsp;&nbsp;if theHttpResponse and isinstance(theHttpResponse, HttpResponse):<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;theHttpResponse['Access-Control-Allow-Origin'] = '*'<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;theHttpResponse['Access-Control-Max-Age'] = '120'<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;theHttpResponse['Access-Control-Allow-Credentials'] = 'true'<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;theHttpResponse['Access-Control-Allow-Methods'] = 'HEAD, GET, OPTIONS, POST, DELETE'<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;theHttpResponse['Access-Control-Allow-Headers'] = 'origin, content-type, accept, x-requested-with'<br />
&nbsp;&nbsp;&nbsp;&nbsp;return theHttpResponse<br />
    </code>
</blockquote>
From
<b>
    <a href="https://github.com/jzerbe/storagebin/commit/5d0f907be72c6b3cb9351e5e2e2e26c78918e3e1#L0R52">
        storagebin/__init__.py
    </a>
</b>
<br />

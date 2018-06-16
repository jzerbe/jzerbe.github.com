---
layout: default
permalink: /search.html
title: search
---

<div data-role="page">
    {% include header.html %}

    <div data-role="content">
        <script>
          (function() {
            var cx = '014722425947596152422:uikm2inm8ae';
            var gcse = document.createElement('script');
            gcse.type = 'text/javascript';
            gcse.async = true;
            gcse.src = 'https://cse.google.com/cse.js?cx=' + cx;
            var s = document.getElementsByTagName('script')[0];
            s.parentNode.insertBefore(gcse, s);
          })();
        </script>
        <gcse:search></gcse:search>
    </div>
</div>

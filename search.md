---
layout: default
permalink: /search.html
title: search
---

<div data-role="page">
    {% include header.html %}

    <div data-role="content">
        <div id="cse" style="width: 100%;">Loading</div>
        <script src="http://www.google.com/jsapi" type="text/javascript"></script>
        <script type="text/javascript">
            google.load('search', '1', {language : 'en', style : google.loader.themes.MINIMALIST});
            google.setOnLoadCallback(function() {
                var customSearchOptions = {};
                var customSearchControl = new google.search.CustomSearchControl('014722425947596152422:uikm2inm8ae', customSearchOptions);
                customSearchControl.setResultSetSize(google.search.Search.FILTERED_CSE_RESULTSET);
                customSearchControl.draw('cse');
                customSearchControl.setLinkTarget(google.search.Search.LINK_TARGET_SELF);
            }, true);
        </script>
    </div>
</div>

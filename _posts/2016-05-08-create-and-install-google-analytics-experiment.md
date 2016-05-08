---
layout: post
title: create and install a Google Analytics experiment
tags:
- user experience
- Google Analytics
- experiment
- JavaScript
published: true
---
Google Analytics offers an amazing service for free called _Experiments_.
Say I want to figure out which button copy leads to a better conversion?
The logic to choose options and track which one converts best is all hosted by Google Analytics.

First read the crash course on setting up one of these experiments for serving up variations
with JavaScript: [_Content Experiments_](https://developers.google.com/analytics/solutions/experiments-client-side)

I ran through these steps twice, once for our dev/staging property and once for our production property.
In `analytics.rb` I then have a switch:

    ExperimentButton = {
      id: Rails.env.production? ? 'prod_exp_id' : 'dev_exp_id',
    }

To load the experiment in `button.html.haml`:

    - # 1. load particular experiment - allow use of chooseVariation for this experiment
    %script(src="//www.google-analytics.com/cx/api.js?experiment=#{ExperimentButton[:id]}")
    
    // first should always be 'original'
    :javascript
        var button_copy_variations = ['Click Me', 'Click Me Please', 'Click ME NOW'];
        
        // 2. choose variation for user
        // https://developers.google.com/analytics/devguides/collection/gajs/experiments#cxjs-choose
        var button_copy_variation = cxApi.chooseVariation('#{ExperimentButton[:id]}');
        
        $(function() {
            // load chosen variations
            $('#button_call_to_action').text(button_copy_variations[button_copy_variation]);
        });
    
    = link_to 'Click Me', button_submission_path, class: 'btn btn-primary', id: 'button_call_to_action'

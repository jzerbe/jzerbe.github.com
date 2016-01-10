---
layout: post
title: Capybara attach_file on Windows
tags:
- Capybara
- attach_file
- rails
- Windows
published: true
---
[Capybara](https://github.com/jnicklas/capybara) `2.4.4` will fail silently on Windows when trying to use
an otherwise valid (Unix) file path with
[`attach_file`](http://www.rubydoc.info/github/jnicklas/capybara/Capybara%2FNode%2FActions%3Aattach_file).

Based on [_slash and backslash in Ruby_](http://stackoverflow.com/questions/7173000/slash-and-backslash-in-ruby),
I put the following in my `spec_helper.rb`:

    USING_WINDOWS = !!((RUBY_PLATFORM =~ /(win|w)(32|64)$/) || (RUBY_PLATFORM=~ /mswin|mingw/))

In `some_file_upload_spec.rb`:

    file_to_attach = File.join(Rails.root, 'spec/fixtures/files/cool_avatar.jpg')
    if USING_WINDOWS
        file_to_attach = file_to_attach.gsub('/', '\\')
    end
    page.attach_file('file_input_dom_id', file_to_attach)

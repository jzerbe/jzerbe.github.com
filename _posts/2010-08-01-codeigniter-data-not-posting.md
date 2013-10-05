---
layout: post
title: CodeIgniter data not posting
tags:
- CodeIgniter
published: true
---
I have been working in this PHP <a href="http://en.wikipedia.org/wiki/Model–View–Controller"><abbr title="Model View Controller">MVC</abbr></a>
framework called <a href="http://codeigniter.com/">CodeIgniter</a> for the past few weeks. I learned yesterday,
that if some of your post data is missing. It is most likely because CodeIgniter auto-closed your form due to bad markup.
I have something like the following:
<blockquote>&lt;fieldset&gt;<br />
    &lt;?php echo form_open('somecontroller/some_function'); ?&gt;<br />
    ... some form fields ....<br />
    &lt;/fieldset&gt;<br />
    &lt;fieldset&gt;<br />
    ... some more form fields ...<br />
    &lt;/fieldset&gt;<br />
    &lt;?php echo form_close(); ?&gt;</blockquote>
Guess who wasn't getting all of the data posted? This guy! CI auto-closed the form at the first
&lt;/fieldset&gt; tag. Remember kids, always open your forms and close your form outside of any
tags that might close before the form closes. Like this:
<blockquote>&lt;?php echo form_open('somecontroller/some_function'); ?&gt;<br />
    &lt;fieldset&gt;<br />
    ... some form fields ....<br />
    &lt;/fieldset&gt;<br />
    &lt;fieldset&gt;<br />
    ... some more form fields ...<br />
    &lt;/fieldset&gt;<br />
    &lt;?php echo form_close(); ?&gt;</blockquote>

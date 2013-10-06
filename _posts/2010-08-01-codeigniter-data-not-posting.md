---
layout: post
title: CodeIgniter data not posting
tags:
- CodeIgniter
published: true
---
If some of your POST data is missing, it is most likely because [CodeIgniter](http://codeigniter.com/)
auto-closed your form due to bad markup.

I have something like the following:

    <fieldset>
    <?php echo form_open('somecontroller/some_function'); ?>
    ... some form fields ....
    </fieldset>
    <fieldset>
    ... some more form fields ...
    </fieldset>
    <?php echo form_close(); ?>

Guess who wasn't getting all of the data posted? This guy! CI auto-closed the form at the first
`<fieldset>` tag. Remember kids, always open your forms and close your form outside of any
tags that might close before the form closes. Like this:

    <?php echo form_open('somecontroller/some_function'); ?>
    <fieldset>
    ... some form fields ....
    </fieldset>
    <fieldset>
    ... some more form fields ...
    </fieldset>
    <?php echo form_close(); ?>

ehbs.js
=======

ehbs.js is a require.js plugin that can load both uncompiled and precompiled Ember Handlebars templates on demand. It depends on require.js and text.js so make sure you have them in place first.

Here is an example to demonstrate how to use it, in your require config, you setup ehbs like this:

```
require.config({
	baseUrl: '/js/libs',
  ehbs: {
    compile: true,
    extension: '.htm'
  },
  paths: {
    templates: '/templates',
    app: '/js/apps/adminApp'
  },
  shim: {
    'bootstrap': ['jquery'],
    'ember': ['handlebars', 'jquery'],
    'ember-data': ['ember'],
    'ember-validations': ['ember'],
    'utils': ['jquery', 'jquery.cookie'],
    'app': ['ember', 'bootstrap']
  }
});

require(['utils', 'app'], function(utils, App) {
  if (!utils.goodToGo()) {
    return window.location.href = 'upgrade';
  } else {
    return App.start();
  }
});
```
Note in this case the ehbs.js is placed into the baseUrl directory '/js/libs/ehbs.js'.

There are only two options 'compile' and 'extension'.

If compile set to true ehbs will load your uncompiled template, compile it, and load it into Ember.TEMPLATES. On the other hand, if it was set to false, ehbs will load your precompiled template as a js file. In this mode, I recommend you to precompile your templates with node-ember-precompile or anything similar that loads Ember.TEMPLATES automatically, just note that in node-ember-precompile it doesn't really deal with Ember Components, nor any extensions other than '.hbs' or '.handlebars'.

The options extension is for locating your uncompiled templates, remember to include dot for it. And in this example, all my uncompiled templates are something like 'header.htm', 'posts.htm', 'post.edit.htm' etc. It will follow Ember standard and convert dots in file names to '/', e.g. 'post.edit.htm' will be added as Ember.TEMPLATES['post/edit'].

And then to use ehbs to load your template, you simply do

```
define(["ehbs!templates/user/login"], function() {
	//your stuff
});
```
It will take care of loading your template to Ember.TEMPLATES['login'].

Remember to let ehbs work as expected, you need to place your templates like this

project/templates/user/login.htm
project/templates/user/login.js

Where login.htm is the uncompiled version and login.js is the precompiled version. You don't need login.js if you set ehbs.compile to true.

A side note: ehbs.js currently hacks for Ember components, if your template url path contains 'components' in the string, it will always add 'components/' to the template name, no matter where the 'components' is in the url. For example, if you have

project/templates/user/components/side-bar.htm

It will load the template into Ember.TEMPLATES['components/side-bar'].

Hope it helps.


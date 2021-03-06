# Simple Sitemap

This is a simple multidomain sitemap module that allows an easy way of adding pages to a sitemap, with per-page configuration. Additionally, the package features a couple additional goodies not found in other sitemap packages: it automatically handles multiple sitemaps for sites over 50,000 pages, and automatically submits sitemaps to both Google and Bing.

This module builds upon the "simple-sitemap" module from Justin Davis (http://justindavis.co) [Big Thanks!]. If you don't need the multidomain support (example.com, example.de, ...) this module works but I would recommend sticking with the simple-sitemap module.

### Installation

`npm install multidomain-simple-sitemap`

### Usage

The constructor takes four arguments, your site's root domain, an distribution path and an optional dryRun param. If you don't specify a dryRun param, the sitemaps will be submitted to all search engines. Optionally you can pass the top-level domain to make a multidomain sitemap.

~~~javascript

var Sitemap = require('multidomain-simple-sitemap');

// Set up the Sitemap module

var dryRun = true; // only create sitemaps without submitting to search engines
var tld = 'com';
var sitemap = new Sitemap("www.yourdomain."+tld, "/dist", dryRun, tld);

// Add a page to the sitemap

sitemap.add("http://www.yourdomain.com/page.html");

// Flush the sitemap to disk and submit to Bing and Google

sitemap.flush(function(){
    //whatever you do next
});
~~~

### Methods

The module has the following methods:

`.add(url, changeFrequency, priority` - Adds a URL to the sitemap, optionally setting the change frequency and priority for the page. Arguments for the method are:

* `url` (String | required) - the absolute URL for the page you're adding.
* `changeFrequency` (String | optional, default: `"Daily"`) - change frequency for the page. Check [sitemaps.org](http://www.sitemaps.org/protocol.html) for valid values.
* `priority` (Float | optional, default: `0.5`) - a number between 0 and 1.0 indicating the priority of this URL. Check [sitemaps.org](http://www.sitemaps.org/protocol.html) for more details on this number.

`.flush(callback)` - writes the sitemap to disk and submits to Google and Bing. Generally, you'll want to call this once you're finished adding URLs to the sitemap. This method is used internally to automagically handle writing sitemaps to disk in the case of sites with more thatn 50,000 pages.

Takes a callback that will run when it's finished submitting to Google and Bing.

### Other Tidbits of Interest

Sitemaps are named with a count appended to the end of the filename by default. For example, most small sites will end up with a sitemap called `sitemap-01-tld.xml`. This appended value will increase based on the number of sitemaps automatically created.

There is also an `sitemap-index-tld.xml` generated which then is used for the submission to Google and Bing. Make sure this is reachable on your server.
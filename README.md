# node-simple-crawler
Simple web-crawler for nodejs

# Installation
Using npm:

		$ npm install node-spider

# Usage
```js
var Crawler = require('node-simple-crawler');

var crawler = new Crawler({
	concurrent: 5,
	logs: true,
	request: { // config for 'request' package
	  headers: { 'user-agent': 'node-crawler' },
	},
  decodeTo: 'utf8',
  decodeFrom: '', // e.g. 'win-1251'. If defined 'encodeTo' and not 'encodeFrom' then charset of 'response.body' will be detected automatically
	callback: function (err, visited) {
	  if (err) { throw err; }

	  // all requests are done
	  console.log('Done! Visited links:');
	  console.log(visited);
	}
});

var handleRequest = function (err, url, response, $) {
  if (err) {
    console.log('Error url: '+ url);
    throw err;
  }
  // request done

  var that = this;

	console.log(response); // response object
	console.log(url);
	$('a').each(function(index, node) { // cheerio
	  var $node = $(node);
		var href = $node.attr('href').split('#')[0];

		spider.queue(that.resolve(url, href), handleRequest);
	});
};

crawler.queue('http://google.com/', handleRequest);
crawler.queue('http://yahoo.com/', {/* custom options for this link */}, handleRequest);
```
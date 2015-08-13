---
title: Client-Side XSLT Transformations with JavaScript
author: Ivan Zlatev
layout: post
permalink: /2011/05/12/client-side-xslt-transformations-with-javascript/
ratings_users:
  - 1
ratings_score:
  - 3
ratings_average:
  - 3
views:
  - 6962
dsq_thread_id:
  - 302204622
categories:
  - Coding
tags:
  - Dojo
  - HowTo
  - JavaScript
---
I bet you haven&#8217;t ever thought of doing XSLT transformations inside the Web Browser on the client-side, but anyway it&#8217;s absolutely possible (cross-browser) and it performs pretty well as well, which shouldn&#8217;t be a big surprise since the browser is optimized to work with markup.

Real-life practical applications of this however I haven&#8217;t thought of yet, but possibly:

  1. Use XSLT to consume old XML-based web services and convert the XML into JSON on the client side on the fly. Come on, I know you want to! Why do it properly when you can do it in JavaScript?
  2. Create a JavaScript-only  CMS that loads Word documents directly and uses XSLT to display HTML (.docx is zipped xml). Again, why do it properly when you can do it in JavaScript?
  3. For fun and zero profit!

Here is the XSLT transformer JavaScript class and sample. The class uses the Dojo JavaScript framework, but only for the browser checks and AJAX requests, so you can easily replace those with JQuery if you wish. Take it under the MIT/X11 license. There is also a [useless demo here][1].

*Sample*:

<pre class="brush: jscript; title: ; notranslate" title="">// to transform
var xslTransform = new XslTransform("/path/to/sample.xsl");
var outputText = xslTransform.transform("/path/to/sample.xml");
</pre>

*XslTransform.js*:

<pre class="brush: jscript; title: ; notranslate" title="">dojo.provide("XslTransform");

dojo.declare("XslTransform", [],
{
	_xslDoc : null,
	_xslPath : null,

	constructor : function(xslPath) {
		this._xslPath = xslPath;
	},

	transform : function(xmlPath) {
		if (this._xslDoc === null)
			this._xslDoc = this._loadXML(this._xslPath);

		var result = null;
		var xmlDoc = this._loadXML(xmlPath);

		if (dojo.isIE) {
			result = xmlDoc.transformNode(this._xslDoc);
		} else if(typeof XSLTProcessor !== undefined) {
			xsltProcessor = new XSLTProcessor();
	  		xsltProcessor.importStylesheet(this._xslDoc);

	  		var ownerDocument = document.implementation.createDocument("", "", null);
	  		result = xsltProcessor.transformToFragment(xmlDoc, ownerDocument);
		} else {
			alert("Your browser doesn't support XSLT!");
		}

		return result;

	},

	createXMLDocument : function(xmlText) {
		var xmlDoc = null;

		if (dojo.isIE) {
			xmlDoc = new ActiveXObject("Microsoft.XMLDOM");
			xmlDoc.async = false;
			xmlDoc.loadXML(xmlText);
		} else if (window.DOMParser) {
			parser = new DOMParser();
			xmlDoc = parser.parseFromString(xmlText, "text/xml");
		} else {
			alert("Your browser doesn't suppoprt XML parsing!");
		}

		return xmlDoc;
	},

	// Synchronously loads a remote xml file
	_loadXML : function(xmlPath) {
		var getResult = dojo.xhrGet({
			url : xmlPath,
			handleAs : "xml",
			sync: true
		});

		var xml = null;
		// Returns immediately, because the GET is synchronous.
		var xmlData = getResult.then(function (response) {
			xml = response;
		});

		return xml;
	}
});

</pre>

 [1]: {{ site.url }}/files/demos/js-xslt/demo.html
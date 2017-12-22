---
title: "Parse URL with Node.js Legacy API: \"url\""
tags: [nodejs]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: nodejs_url_parse.html
folder: nodejs
toc: false
---

The `url` module provides utilities for URL resolution and parsing. It can be accessed using:

```
const url = require('url');
```

See details in the reference listed below.

## Exmaple of URL Parser in Javascript

```js
const url = require("url");

/**
 * This parser will remove the username and password in the URL, if any
 * @param {String} inputURL
 * @return {String}
 */
module.exports.reconstructURL = function(inputURL) {
    const urlObj = url.parse(inputURL);
    let { protocol, host, path, hash } = urlObj;

    protocol = (protocol === null || protocol === undefined) ? "" : protocol + "//";
    host = host || "";
    path = path || "";
    hash = hash || "";

    return protocol + host + path + hash;
};
```

## Note

* This `url` module doesn't try to link to the URL which it parses. It doesn't check if the `protocol` is valid, or if the `host` is missing, or if the `host` or `pathname` leads to an existent resource.

* If any component (i.e. `protocol`, `host`, `pathname`, `hash`, etc.) of the URL is missing, this module will return a `null` for this component, not `undefined`.

* If the `host` of the URL doesn't have a final slash, this module will add a final slash to it.

  If the `pathname` of the URL doesn't have a final slash, the module will keep it as it was.
  ```
  https://www.thisisafakehost.com ==> https://www.thisisafakehost.com/
  https://www.thisisafakehost.com/trythis ==> https://www.thisisafakehost.com/trythis
  ```

## Reference

* [Legacy URL API of Node.js](https://nodejs.org/docs/latest/api/url.html#url_legacy_url_api)

{% include links.html %}
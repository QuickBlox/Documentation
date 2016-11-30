The QuickBlox JavaScript SDK provides a JavaScript library making it even easier to access the QuickBlox cloud communication backend platform.
Our goal is let you quickly start building your JavaScript app on Quickblox.

<span id="Overview" class="on_page_navigation"></span>
## Overview

The latest version is [**2.5.0**](https://github.com/QuickBlox/quickblox-javascript-sdk/releases/latest).
We are using [Github Releases](https://github.com/QuickBlox/quickblox-javascript-sdk/releases) to make changelog.

Also we have [SDK API Reference](http://quickblox.github.io/quickblox-javascript-sdk/docs/).

JS SDK supports Chrome 30+, Safari 7.1+, Firefox 30+, IE 10+, Opera 23+, Edge 12+ and Node v.4+.

You can check JavaScript SDK by running tests:
* Visit our [a Spec Runner page on GitHub](https://quickblox.github.io/quickblox-javascript-sdk/spec/SpecRunner.html).
* Or download the Javascript SDK, you will find the Spec Runner in the `/spec` folder and you can run by [Jasmine](https://jasmine.github.io/).

<span id="Install" class="on_page_navigation"></span>
## Install 
JS SDK supports [UMD (Universal Module Definition)](https://github.com/umdjs/umd) pattern for JavaScript modules. So it is possible to use SDK everywhere (as browser global variable, with AMD module loader like RequireJS or as CommonJS module).

> If you are using JS SDK version less 2.5.0 you need to include either [jQuery](http://jquery.com/download) or [Zepto](http://zeptojs.com/) in your html before our SDK.

There are a few ways to get started with JS SDK.

#### CDN
The simplest way to load the JS SDK is to add a &lt;script&gt; tag:
```html
<script src='https://unpkg.com/quickblox@2.4.0/quickblox.min.js'></script>
```

> You also can use another content delivery network, for example [cdnjs](https://cdnjs.com/libraries/quickblox).

And SDK will be avaible via the `QB` namespace.

```javascript
QB.init(appId, authKey, authSecret);
```

#### NPM
The JS SDK is also available on [NPM](https://www.npmjs.com/package/quickblox):

```bash
npm install quickblox --save
```

JS SDK can then be accessed by requireing the module:

```javascript
var QB = require('quickblox');
```

#### Bower

```bash
bower install quickblox --save
```

#### Download sources
Also, you can download [sources from Github (zip archive)](https://github.com/QuickBlox/quickblox-javascript-sdk/archive/gh-pages.zip) and use as you like.

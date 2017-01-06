The QuickBlox JavaScript SDK provides a JavaScript library making it even easier to access the QuickBlox cloud communication backend platform.

Our goal is let you quickly start building your JavaScript app on Quickblox.

<hr>

<span id="Overview" class="on_page_navigation"></span>
## Overview

The latest version of JS SDK is [**2.5.0**](https://github.com/QuickBlox/quickblox-javascript-sdk/releases/latest),
a full list of JS SDK versions can be found on [Github Releases](https://github.com/QuickBlox/quickblox-javascript-sdk/releases).

Check out our [API Reference](http://quickblox.github.io/quickblox-javascript-sdk/docs/) for more detailed information about our SDK.

JS SDK supports Chrome 30+, Safari 7.1+, Firefox 30+, IE 10+, Opera 23+, Edge 12+ and Node v.4+.

You can check JavaScript SDK by running tests:
* Visit our [a Spec Runner page on GitHub](https://quickblox.github.io/quickblox-javascript-sdk/spec/SpecRunner.html).
* Or download the Javascript SDK, you will find the Spec Runner in the `/spec` folder and you can run by [Jasmine](https://jasmine.github.io/).

<span id="Install" class="on_page_navigation"></span>
## Install

JS SDK supports [UMD (Universal Module Definition)](https://github.com/umdjs/umd) pattern for JavaScript modules. So it is possible to use SDK everywhere (as browser global variable, with AMD module loader like RequireJS or as CommonJS module).

<div class="panel panel-warning">
    <div class="panel-body">
        If you are using JS SDK version less 2.5.0 you need to include either <a href="http://jquery.com/download" target="_blank">jQuery</a> or <a href="http://zeptojs.com/" target="_blank">Zepto</a> in your html before our SDK, only for browser environment.
    </div>
</div>

There are several ways to get started with JS SDK.

#### CDN

The simplest way to load the JS SDK is to add a &lt;script&gt; tag:
```html
<script src='https://unpkg.com/quickblox@x.x.x/quickblox.min.js'></script>
```

<div class="panel panel-info">
    <div class="panel-body">
        You also can use another content delivery network, for example <a href="https://cdnjs.com/libraries/quickblox" target="_blank">cdnjs</a>.
    </div>
</div>

And SDK will be avaible via the `QB` namespace.

```javascript
QB.init(appId, authKey, authSecret);
```

#### NPM

The JS SDK is also available on [NPM](https://www.npmjs.com/package/quickblox):

```bash
npm install quickblox --save
```

JS SDK can then be accessed by requiring the module:

```javascript
var QB = require('quickblox');
```

#### Bower

```bash
bower install quickblox --save
```

#### Download sources

You can also download [sources from Github (zip archive)](https://github.com/QuickBlox/quickblox-javascript-sdk/archive/gh-pages.zip) and use as you like.

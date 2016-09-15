The latest version is 2.2.1. 

<span id="Dependencies_for_browser" class="on_page_navigation"></span>
# Dependencies for browser

For the library to work, you need to include either [jQuery](http://jquery.com/download) or [Zepto](http://zeptojs.com/) in your html before `quickblox.min.js`, like so:

```javascript
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/quickblox/1.17.2/quickblox.min.js"></script>
```

<span id="Bower_and_RequireJs" class="on_page_navigation"></span>
# Bower and RequireJs

If you use bower package manager for your project, you can install JS SDK through bower:

```bash
bower install quickblox --save
```

When you use [RequireJS](http://requirejs.org), you are able to use quickblox as AMD module. SDK supports [UMD (Universal Module Definition)](https://github.com/umdjs/umd) pattern for JavaScript modules. So it is possible to use SDK everywhere (as browser global variable, with AMD module loader like RequireJS or as CommonJS module for Node.js environment).

<span id="NodeJs_and_NPM_integration" class="on_page_navigation"></span>
# NodeJs and NPM integration

Also you can use QuickBlox JavaScript SDK with server-side applications on NodeJS through the native node package. Just install the package in your application project like that:

```bash
npm install quickblox --save
```

And you're ready to go:

```javascript
var QB = require('quickblox');
	 
// OR to create many QB instances
var QuickBlox = require('quickblox').QuickBlox;
var QB1 = new QuickBlox();
var QB2 = new QuickBlox();
```

<span id="Download_ZIP_archive" class="on_page_navigation"></span>
# Download ZIP archive
[QuickBlox JavaScript SDK, zip archive](https://github.com/QuickBlox/quickblox-javascript-sdk/archive/gh-pages.zip)

<span id="Browsers_support" class="on_page_navigation"></span>
# Browsers support
| IE   | Firefox | Chrome | Safari | Opera | Android Browser | Blackberry Browser | Opera Mobile | Chrome for Android | Firefox for Android |
|:----:|:-------:|:------:|:------:|:-----:|:---------------:|:------------------:|:------------:|:------------------:|:------------------------:|
| 10+  |  4+     | 13+    |  6+    |  12+	 |       4.4+	     |         10+        |	     12+	    |         35+        |	         30+             |

<span id="Library_changelog" class="on_page_navigation"></span>
# Library changelog
#### 1.17.2 — Dec 7, 2015

* Fixed an issue with WebRTC Video calling module when `QB.init(sessionToken, appId)` is used.
<br>

#### 1.17.1 — Dec 7, 2015

* Added `QB.chat.onMessageErrorListener(messageId, error)` to track errors in chat.
* Added an ability to init SDK via `QB.init(sessionToken, appId)`.
<br>

<details>
  <summary>Previous versions</summary>
 
  
#### 1.17.0 — Dec 4, 2015

* Privacy Lists API
* Now `QB.data.uploadFile` works on node.js. The method `QB.data.updateFile was` deleted.
* Added tests via Jasmine for all modules

</details>
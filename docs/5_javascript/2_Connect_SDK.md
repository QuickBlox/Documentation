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

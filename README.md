# node-sass-endpoint

Easily serve a SASS file as CSS from an express endpoint. No grunt/gulp, no build files, no required configuration – just pure data.

### Dependencies

- [node-sass](https://www.npmjs.com/package/node-sass) (taken care of by npm install)
- ES6 `Object.assign` (either use node v4.0+ or a [polyfill](https://www.npmjs.com/package/es6-object-assign))

### Installation

    $ npm install node-sass-endpoint --save

## Usage - Easy Version

Assuming you have the following directory structure:

```
client/
├── app.scss
└── index.html


server/
└── index.js

package.json
```

Then you can write the following as your `server/index.js`:

```javascript
// server.js
var express = require('express');
var sass = require('node-sass-endpoint');
var app  = express();

// Serve CSS
app.get('/assets/app-bundle.css',
  sass.serve('./client/app.scss'));

// Serve HTML
var path = require('path')
app.get('/', function (req, res) {
  res.sendFile( path.resolve(__dirname, '../client/public/admin.html') )
})

console.log("Listening on port 5555...");
app.listen(5555);
```

And run `node server/index.js`.

Now any GET request to `localhost:5555/assets/app-bundle.css` will compile and serve the SASS file located at `./client/app.scss`. Any `@import` statements within `app.scss` will also be included in the final output.

With that said, here is an example of using your sass-compiled CSS in `client/index.html`:

```html
<html>
<head>
    <link rel="stylesheet" href="/assets/app-bundle.css">
</head>
<body>
    <!-- page content -->
</body>
</html>
```

## Advanced Usage

```javascript
app.get(
  '/assets/app-bundle.css',
  sass.serve('./client/app.scss', {

    // (dev only) defaults to parent folder of scss file.
    // Any sass file changes in this directory will clear the output cache.
    watchDir: './client/',

    // Defaults to parent folder of scss file.
    // The node_modules/ is always included.
    includePaths: ['./client/'],

    // Defaults to "nested". Can be one of: "nested", "expanded", "compact", or "compressed".
    outputStyle: 'compressed',

    // Defaults to false
    debug: false
  })
);
```

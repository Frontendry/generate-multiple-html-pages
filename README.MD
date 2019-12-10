## How to generate multiple HTMLs using html-webpack-plugin:

See the commented code below to do it:

### Project Structure

```
application-folder-structure
|--dist
|------assets
|----------css
|----------imgs
|----------js
|---------------main.js //bundled js
|------about.html // Generated about.html file
|------index.html // Generated index.html file
|--node_modules
|--src
|------assets
|------imgs
|------js
|----------app.js
|------scss
|------html
|----------partials
|----------about.ejs
|----------index.ejs
```

### webpack.config.js

```javascript
// webpack.config.js

...

const path = require("path");
const fs = require("fs"); // A native module for effectively working with files built on top of Node's famous fs module. The fs module comes with Node so it is already available to you.
const HtmlWebpackPlugin = require("html-webpack-plugin");

const multipleHtmlsWebpackPlugin = function() {
    // Use fs to read your html templates folder
  let dirents = fs.readdirSync(path.join(__dirname, "src/html"), {
    withFileTypes: true
  });

    // Get files only and remove subfolders(partials folder in this case)
  let templateFiles = dirents.filter(dirent => !dirent.isDirectory());

    // Go through each file and return the dynamic HtmlWebpackPlugin config
  let templateFilesGenerateHtmls = templateFiles.map(function(templateFile) {
      // Split the file into:
    let parts = templateFile.name.split(".");

    // its file name
    let name = parts[0];

    // its extension
    let extension = parts[1];

    // Fix the above values into the HtmlWebpackPlugin config
    return new HtmlWebpackPlugin({
        template: `${path.join(__dirname, "src/html/")}${name}.${extension}`,
        filename: `../../${name}.html`, // the generated HTML file path is relative to the outputted js(bundled js)
        inject: true,
        title: "HTML Page Title"
    });
  });

  // Return the generated HtmlWebpackPlugin config
  return templateFilesGenerateHtmls;
};

let webpackConfig = {
    entry: "./src/app.js",
    output: {
        path: path.join(__dirname, "dist/assets/js"),
        filename: "main.js",
        chunkFilename: "[name].js",
        publicPath: "/assets/js/"
    },

    ...

    plugins: [
        ...

    ].concat(multipleHtmlsWebpackPlugin())
}


module.exports = webpackConfig;


```

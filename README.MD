## How to use:

## Project Structure

sample-application-structure
|--dist
|------assets
|----------js
|---------------main.js*(Bundled js)*
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

```javascript
// webpack.config.js

...
const path = require("path");
const fs = require("fs");
const HtmlWebpackPlugin = require("html-webpack-plugin");

const multipleHtmlsWebpackPlugin = function() {
  let dirents = fs.readdirSync(path.join(__dirname, "src/html"), {
    withFileTypes: true
  });

    // Get files only and remove subfolders
  let templateFiles = dirents.filter(dirent => !dirent.isDirectory());

  let templateFilesGenerateHtmls = templateFiles.map(function(templateFile) {
    let parts = templateFile.name.split(".");
    let name = parts[0];
    let extension = parts[1];

    return new HtmlWebpackPlugin({
      template: path.join(__dirname, "src/html/") + name + "." + extension,
      filename: "../../" + name + ".html", // the generated HTML file path is inregards to the outputted js(bundled js)
      inject: true,
      title: "HTML Page Title"
    });
  });
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

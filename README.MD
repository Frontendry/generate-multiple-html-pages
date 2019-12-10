##How to use:

```javascript
// webpack.config.js

...

const fs = require("fs");
const HtmlWebpackPlugin = require("html-webpack-plugin");


const multipleHtmlsWebpackPlugin = function() {
  let dirents = fs.readdirSync(path.join(__dirname, "src/html"), {
    withFileTypes: true
  });

  let templateFiles = dirents.filter(dirent => !dirent.isDirectory());

  let templateFilesGenerateHtmls = templateFiles.map(function(templateFile) {
    let parts = templateFile.name.split(".");
    let name = parts[0];
    let extension = parts[1];

    return new HtmlWebpackPlugin({
      template: path.join(__dirname, "src/html/") + name + "." + extension,
      filename: "../../" + name + ".html", // the generated HTML file path is inregards to the outputted js
      inject: true,
      title: "HTML Page Title"
    });
  });
  return templateFilesGenerateHtmls;
};

let webpackConfig = {

    ...

    plugins: [
        ...
    ].concat(multipleHtmlsWebpackPlugin())
}


module.exports = webpackConfig;


```

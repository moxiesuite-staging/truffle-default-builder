# moxie-default-builder

This is the build system that came standard with Truffle 1.0 and 2.0. For Truffle 3.0, it has been separated out as its own module and is no longer shipped as the default.

# Features

The default builder comes with a few standard niceties meant to get you started quickly:

* Automatic bootstrapping of your application within the browser, importing your compiled contract artifacts, deployed contract information and Ethereum client configuration automatically.
* Inclusion of recommended dependencies, including [web3](https://github.com/ethereum/web3.js/tree/master) and [Ether Pudding](https://github.com/ConsenSys/ether-pudding).
* Support for ES6 and JSX built-in.
* SASS support for manageable CSS.
* And UglifyJS support for creating minified versions of your Javascript assets.

# Configuration

The default builder gives you complete control over how you want to organize your files and folders, but the default directory structure looks like this:

```none
app/
- javascripts/
  - app.js
- stylesheets/
  - app.css
- images/
- index.html
```

It's corresponding configuration within your [project configuration](/docs/advanced/configuration) looks very similar:

```javascript
{
  "build": {
    // Copy ./app/index.html (right hand side) to ./build/index.html (left hand side).
    "index.html": "index.html",

    // Process all files in the array, concatenating them together
    // to create a resultant app.js
    "app.js": [
      "javascripts/app.js"
    ],

    // Process all files in the array, concatenating them together
    // to create a resultant app.css
    "app.css": [
      "stylesheets/app.scss"
    ],

    // Copy over the whole directory to the build destination.
    "images/": "images/"
  }
}
```

This configuration describes "targets" (left hand side) with files, folders and arrays of files that make up the targets' contents (right hand side). Each target will be produced by processing the files on the right hand side based on their file extension, concatenating the results together and then saving the resultant file (the target) into the build destination. Where a string is specified on the right hand side instead of an array, that file will be processed, if needed, and then copied over directly. If the string ends in a "/", it will be interpreted as a directory and the directory will be copied over without further processing. All paths specified on the right hand side are relative to the `app/` directory.

You can change this configuration and directory structure at any time. You aren't required to have a `javascripts` and `stylesheets` directory, for example, but make sure you edit your configuration accordingly.

**Special note:** If you want the default builder to bootstrap your application on the frontend, make sure you have a build target called `app.js` which the default builder can append code to. It will not bootstrap your application with any other filename.

# Usage in Truffle 3.0

To use in Truffle 3.0, first install the default builder as a dependency of your project:

```
$ npm install truffle-default-builder --save
```

Next, use the default builder within your Truffle configuration by `require`'ing it and passing a new instance of the builder into Truffle:

```javascript
var DefaultBuilder = require("truffle-default-builder");

module.exports = {
  build: new DefaultBuilder({
    "index.html": "index.html",
    "app.js": [
      "javascripts/app.js"
    ],
    "app.css": [
      "stylesheets/app.css"
    ],
    "images/": "images/"
  }),
  networks: {
    development: {
      host: "localhost",
      port: 8545,
      network_id: "*" // Match any network id
    }
  }
};
```

# Build Artifacts

Your build artifacts are saved within the `./build` directory, along side compiled deployed contract artifacts in `./build/contracts`.

# Considerations

The default builder is easy to use for most projects, and allows you to quickly get started. However, it has some drawbacks:

* It doesn't currently support `import`, `require`, etc., provided to you by tools like Browserify, Webpack, and CommonJS, making dependency management harder than it should be.
* It's a custom build system and doesn't easily integrate with other popular build systems.
* It is extensible, but again, using custom methods and APIs.

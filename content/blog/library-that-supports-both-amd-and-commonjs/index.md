---
title: [Draft] Create a library and support both AMD and commonjs.
date: "2019-09-29T14:00:00.001Z"
description: ""
---

- [AMD](https://en.wikipedia.org/wiki/Asynchronous_module_definition)
- [CommonJS](https://en.wikipedia.org/wiki/CommonJS)

```javascript
// ./src/index.js

function something() {
    // This allows us to lazy load other modules in.
    const f = import('/another-module');
    f.what();
}

export {
    something,
}

```

```javascript
// webpack.config.js
function createConfig(target) {
  return {
    entry: './src/index.js',
    output: {
      filename: `./${target}/index.js`,
      chunkFilename: `./${target}/[name].js`,
      libraryTarget: target
    },
    name: target,
    mode: 'production',
  }
}

module.exports = [
  createConfig('amd'),
  createConfig('commonjs'),
]
```
+++ 
title = "WebPack Loader vs Plugin"
description = "The purpose of webpack loader and plugin."
date = "2023-10-30"
author = "Kamesh Sethupathi"
tags = ["javascript", "devtool", "tech"]
+++

## WebPack : Loader VS Plugin
- Though there are couple of bundler available for javascript, Webpack steals the heart of most javascript developers.
- The reason is obviously due to its vast ecosystem of loaders and plugins, which extend its core bundling functionality.
- Even senior developers get confused between the purpose of loader and plugin. **This article is for you if you dont want to be one of them.**

## How WebPack bundles code
- WebPack at it's core is just a module bundler.
- When one file depends on another, webpack treats this as a dependency.
- It starts the bundling process from the given entry files, scans for certain  text within the content like `import` and evaluates them.
- Finds the dependencies from the previous evaluation and add them to the dependency graph.
- Does the same for the dependencies.
- At last it generates one or more output bundles - often, only one.

## WebPack Loaders
- Loaders **work at the individual file level** during or before the bundle is generated.
- Out of the box, webpack only understands JavaScript and JSON files.
- Loaders allow webpack to process other types of files and convert them into valid modules that can be consumed by your application and added to the dependency graph.


## WebPack Plugins
- Plugins **work at bundle or chunk level** and usually work at the end of the bundle generation process. Plugins can also modify how the bundles themselves are created.
- Plugins can deeply integrate into webpack because they can register hooks within webpacks build system and access (and modify) the compiler, and how it works, as well as the compilation.
- Plugins have more powerful control than loaders.
- Plugins can be leveraged to perform a wider range of tasks like bundle optimization, asset management and injection of environment variables.

## Reach me
- 💬 Ask me about Frontend and Backend Technologies 
- 📫 How to reach me: [Twitter @kamesh_koops](https://twitter.com/kamesh_koops)


### Reference
- https://webpack.js.org/concepts
- https://stackoverflow.com/questions/37452402/webpack-loaders-vs-plugins-whats-the-difference
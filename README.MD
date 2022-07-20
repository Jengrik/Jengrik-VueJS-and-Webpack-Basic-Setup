<div align="center">
    <h1>VueJS 2 and Webpack Basic Setup</h1>
    <p>This project presents a basic template with the minimum configuration that VueJS 2 and WebPack should have in the client side. </p>
    <a href="https://github.com/Jengrik/Jengrik-VueJS-and-Webpack-Basic-Setup">
        <img width="600" height="200" src="documentation\description.svg">
    </a>
    <p align="right"><strong>Designed by: Jengrik</strong></p>
</div>

## Table of Contents

1. [Getting Started](#getting-started)
2. [Dependencies Installation](#dependencies-installation)
    - [VueJS](#vuejs)
    - [Webpack](#webpack)
    - [Babel](#babel)
3. [General Configuration](#general-configuration)
    - [VueJS App Configuration](#vuejs-app-configuration)
    - [Webpack Configuration File](#webpack-configuration-file)
    - [Babel Configuration File](#babel-configuration-file)
4. [References](#references)

<br>
<h2 align="center">Getting Started</h2>

NPM (originally short for Node Package Manager) is a package manager for the JavaScript programming language maintained by npm, Inc. 

It consists of a command line client, and an online database of public and paid-for private packages, called the npm registry.

You can initialize the `package.json` file using:

```bash
npm init
```
or quickly without dialogs as:
```bash
npm init --yes
```

After that, you can install packages using the `install` command. To achieve that, you can use:

```bash
npm install <package> <specification>
```
or with its abbreviation as
```bash
npm i <package> <specification>
```

You can control where and how they get saved with some additional flags on the specification field:

- `-P`, `--save-prod` <br>Package will appear in your dependencies. This is the default unless -D or -O are present.

- `-D`, `--save-dev` <br>Package will appear in your *devDependencies*.

- `-O`, `--save-optional` <br>Package will appear in your *optionalDependencies*.

- `--no-save` <br>Prevents saving to dependencies.

When you are using any of the above options to save dependencies to your `package.json`, there are two additional, optional flags:

- `-E`, `--save-exact` <br>Prevents saving to dependencies.

- `-B`, `--save-bundle` <br>Prevents saving to dependencies.

<br>
<h2 align="center">Dependencies Installation</h2>

<h3>VueJS</h3>

Since we will use VueJS as Front-End framework then we will need a Vue loader for the Single-file components in Webpack. First, you should choose the VueJS version that you will use, in this case, I will use VueJS 2.

Vue 2 has shipped a final minor release (2.7) in July 2022, which backports a selected subset of new features from Vue 3. Vue 2 has now entered maintenance mode: it will no longer ship new features but will continue to receive critical bug fixes and security updates until the end of 2023. For this reason, is highly recommended following the next configuration:

```bash
npm i vue@2.7.0 -D
npm i vue-template-compiler@2.7.0 -D
```

The compiler's version must be in sync with the base VueJS package so that `vue-loader` produces code that is compatible with the runtime. __This means, every time you upgrade VueJS in your project, you should upgrade `vue-template-compiler` to match it as well.__

<h3>Webpack</h3>

Webpack is a free and open-source module bundler for JavaScript. It is made primarily for JavaScript, but it can transform front-end assets such as HTML, CSS, and images if the corresponding loaders are included. it will be shown how to install it using NPM.

Install the package:

```bash
npm i webpack -D
```

For a custom webpack project, webpack CLI provides a flexible set of commands increase speed, you can install it using:

```bash
npm i webpack-cli -D
```

You already have realized that is necessary generate a Webpack build each time that you made a change. You can save that time using the `webpack-dev-server` since it provides live reloading.

```bash
npm i webpack-dev-server -D
```

Additionally, `vue-loader` is a loader for webpack that allows you to create Vue components in a format called Single-File Components (SFCs), the last version released for VueJS 2 is the 15, you must use:

```bash
npm i vue-loader@15.9.8 -D
```

In a nutshell, the combination of webpack and vue-loader gives you a modern, flexible and extremely powerful front-end workflow for authoring Vue.js applications. If you want to treat assets referenced in `<style>`  as module dependencies then you need to handle them with webpack loaders. In this case, we will use `css-loader`, you could install it using:

```bash
npm i css-loader -D
```

<h3>Babel</h3>
Once the VueJs and its loaders has been installed, we could start thinking about babel and its integration with webpack. Babel is a tool that helps you write code in the latest version of JavaScript. 

When your supported environments don't support certain features natively, Babel will help you compile those features down to a supported version.

To include babel in our project with Webpack, you need to include the loader `babel-loader`, this package allows transpiling JavaScript files. 

This loader requires a small preset called `babel/preset-env` that allows you to use the latest JavaScript without needing to micromanage syntax. The Babel compiler core is called `@babel/core`. To install those packages, you should use:

```bash
npm i babel-loader @babel/core @babel/preset-env -D
```

<br>
<h2 align="center">General Configuration</h2>

<h3>VueJS App Configuration</h3>

In many Vue projects, global components will be defined using `Vue.component`, followed by `new Vue({ el: '#container' })` to target a container element in the body of every page. In this case, in our JS Entry File we use:

```js
new Vue({
    render: h => h(App)
}).$mount('#bodyVueContainer');
```

One important thing to note is that separation of concerns is not equal to separation of file types. In modern UI development, instead of dividing the codebase into three huge layers that interweave with one another, it makes much more sense to divide them into loosely-coupled components and compose them. Inside a component, its template, logic and styles are inherently coupled, and collocating them actually makes the component more cohesive and maintainable.

For that reason, we will use the basic `Vue.component` definition:

```vuejs
<template>
    <div>-- Content --</div>
</template>
<script src="./logic.js"></script>
<style src="./style.css"></style>
```

<h3>Webpack Configuration File</h3>

Vue Loader's configuration is a bit different from other loaders. In addition to a rule that applies `vue-loader` to any files with extension `.vue`, make sure to add Vue Loader's plugin to your webpack config.

The plugin is is responsible for cloning any other rules you have defined and applying them to the corresponding language blocks in `.vue` files. For example, if you have a rule matching `/\.js$/`, it will be applied to `<script>` blocks in `.vue` files.

The basic rules that you Webpack file need are:

```js
rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                          ['@babel/preset-env', { targets: "defaults" }]
                        ]
                    },
                }
            },
            {
                test: /\.vue$/,
                use: {
                    loader: 'vue-loader'
                }
            },
            {
                test: /\.css$/,
                use: [
                    'vue-style-loader',
                    'css-loader'
                ],
            },
        ]
```

<h3>Babel Configuration File</h3>

`@babel/preset-env` is a smart preset that allows you to use the latest JavaScript without needing to micromanage which syntax transforms (and optionally, browser polyfills) are needed by your target environment(s). This both makes your life easier and JavaScript bundles smaller!

`@babel/preset-env` takes any target environments you've specified and checks them against its mappings to compile a list of plugins and passes it to Babel.

You can include it in your Webpack file using:

```js
use: {
        loader: 'babel-loader',
        options: {
            presets: [
                ['@babel/preset-env', { targets: "defaults" }]
            ]
        },
     }
```
When no targets are specified: Babel will assume you are targeting the oldest browsers possible. For example, @babel/preset-env will transform all ES2015-ES2020 code to be ES5 compatible.

<br>
<h2 align="center">References</h2>

This document is only a guide that aims to present the basic configuration of a simple project. The information was taken from the following sources:

- `NPM` <br>https://docs.npmjs.com/

- `VueJS 2` <br>https://v2.vuejs.org/v2/guide/

- `Webpack 5` <br>https://webpack.js.org/concepts/

- `Babel` <br>https://babeljs.io/docs/en/.
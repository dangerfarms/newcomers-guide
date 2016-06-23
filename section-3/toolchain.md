# Toolchain

We use several build tools and libraries across our frontend projects. These are the main ones you will come across.

## NPM

NPM is the package manager for JavaScript.

You'll need it installed on your system to work on any of our frontend projects. NPM comes bundled with [Node.js](https://dangerfarms.gitbooks.io/newcomers-guide/content/section-3/toolchain.html#nodejs).

Once you have NPM, you will be able to install everything else you need by running `npm install` from inside our projects.

Read more here: https://docs.npmjs.com/

## Node.js

Node.js allows us to run JavaScript outside of the browser.

Most frontend build tools are written in JavaScript and run on the command line, so we need Node.js to run them.

We recommend using [NVM](https://github.com/creationix/nvm) to manage your Node.js install. This makes switching versions easier.

Having said this, we generally make sure our projects work well with the latest version of Node. This means it's fine install it [the usual way](https://nodejs.org/en/), or by using your OS [package manager](https://nodejs.org/en/download/package-manager).

## Webpack

Webpack is a code bundling tool. 

When developing the frontend, we split our code into files and modules. But browsers do not directly support JS modules. And if they did, loading them dynamically would result in lots of network requests and a slow app.

Webpack is an NPM module, so you don't need to install it directly. You'll get it when you run `npm install`.

## Babel

Babel translates ES6 (aka ES2015) into the ES5 code that browsers can run. This process is called "transpiling".

Babel is an NPM module, so you don't need to install it directly. You'll get it when you run `npm install`.

## Sass

Sass adds new features to CSS. The [documentation](http://sass-lang.com/guide) is great, read it if you want to learn more.

Again, this is an NPM module, so don't worry about installing it globally on your system.
# Steps

## Create base repo

```console
$ mkdir my-repo
$ cd my-repo
$ pwd
.../my-repo
$ touch package.json
```

```json
{
  "private": true
}
```

## Install lerna

```console
$ npm install --save-dev lerna
$ npx lerna init
```

You will see `lerna.json` generated.

## Create packages

Create files as:

```
my-repo
+ packages
  + main
  | + index.js
  | + package.json
  + sub
    + index.js
    + package.json
```

`packages/main/index.js`

```js
const sub = require('project-sub');

main();

function main() {
  process.stdout.write(`It says: ${sub.message}\n`);
}
```

`packages/main/package.json`

```json
{
  "private": true,
  "scripts": {
    "start": "node ."
  },
  "dependencies": {
    "project-sub": "0.0.0"
  }
}
```

`packages/sub/index.js`

```js
module.exports = {
  message: "Message from sub package",
};
```

`packages/sub/package.json`

```json
{
  "private": true,
  "name": "project-sub",
  "version": "0.0.0"
}
```

## Connect packages

```console
$ npx lerna bootstrap
```

## Run package scripts

```console
$ npx lerna run start
lerna notice cli v4.0.0
lerna info Executing command in 1 package: "npm run start"
lerna info run Ran npm script 'start' in 'undefined' in 0.3s:

> start
> node .

It says: Message from sub package
lerna success run Ran npm script 'start' in 1 package in 0.3s:
lerna success - undefined
```

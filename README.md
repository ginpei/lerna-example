# Lerna?

>  A tool for managing JavaScript projects with multiple packages.

- https://github.com/lerna/lerna
- https://lerna.js.org/

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

# Pit holes

## Name and version required

You have to define sub package's name and version, and refer them from your main package.

You can set `private: true` flag.

## Name must fulfill npm package name

Valid:

- `"any-package-name"`
- `"@my-local-project/sub"`

Invalid:

- `"@sub"`
- `"@@my-local-project/sub"`

```console
$ npx lerna bootstrap
lerna notice cli v4.0.0
lerna ERR! Error: Invalid package name "@@local/sub": name can only contain URL-friendly characters
lerna ERR!     at invalidPackageName (/home/ginpei/projects/lerna-example/node_modules/npm-package-arg/npa.js:84:15)
lerna ERR!     at Result.setName (/home/ginpei/projects/lerna-example/node_modules/npm-package-arg/npa.js:119:11)
lerna ERR!     at new Result (/home/ginpei/projects/lerna-example/node_modules/npm-package-arg/npa.js:110:10)
lerna ERR!     at Function.resolve (/home/ginpei/projects/lerna-example/node_modules/npm-package-arg/npa.js:54:15)
lerna ERR!     at new Package (/home/ginpei/projects/lerna-example/node_modules/@lerna/package/index.js:95:26)
lerna ERR!     at /home/ginpei/projects/lerna-example/node_modules/@lerna/project/index.js:207:26
lerna ERR! lerna Invalid package name "@@local/sub": name can only contain URL-friendly characters
```

## Version must follow node-semver

- https://github.com/npm/node-semver

Valid:

- `"0.0.0"`
- `"12.345.6789"`
- `"0.0.0-beta"`
- `"0.0.0-local"`

Invalid:

- `"local"`
- `"x"`

```console
$ npx lerna bootstrap
lerna notice cli v4.0.0
lerna info Bootstrapping 2 packages
lerna info Installing external dependencies
lerna ERR! npm install exited 1 in 'undefined'
lerna ERR! npm install stderr:
npm ERR! code E404
npm ERR! 404 Not Found - GET https://registry.npmjs.org/project-sub - Not found
npm ERR! 404 
npm ERR! 404  'project-sub@local' is not in the npm registry.
npm ERR! 404 You should bug the author to publish it (or use the name yourself!)
npm ERR! 404 
npm ERR! 404 Note that you can also install from a
npm ERR! 404 tarball, folder, http url, or git url.

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/ginpei/.npm/_logs/2021-11-07T04_02_09_336Z-debug.log
lerna ERR! npm install exited 1 in 'undefined'
```

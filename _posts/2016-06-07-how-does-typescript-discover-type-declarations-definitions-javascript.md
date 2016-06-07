---
title: "How does TypeScript discover type declarations and definitions for JavaScript code?"
author: Ivan Zlatev
layout: post
categories:
  - Coding
---

In this blog post I will cover how does TypeScript discover type declarations and the subtle differences in what the declarations should look like depending on where they live. 

Edit: Note that Visual Studio IDE, Visual Studio Code and possibly other editors have built their TypeScript/JavaScript intellisense on top of the TypeScript language service (that is - TypeScript itself), so all of the below apply for those as well.

Let's say that we have a JavaScript module like the one below and we want to consume it together with type declarations either in TypeScript or JavaScript (TypeScripts can process JavaScript and provide intellisense via `allowJs`).


## Our JavaScript module code

**hello.js**

```javascript
module.exports.Name = function(first, last) {
  this.first = first;
  this.last = last;
}

module.exports.sayHi = function(name) {
  console.log(```Hi ${name.first} ${name.last}!```);
};
```

We want to consume it in TypeScript and we do this:

**main.ts**

```typescript
import * as hello from 'hello';
```

This will trigger a search for type declarations during the compilation/transpilation phase, so let's go through the various possibilities.

## Tripple slash comments

The legacy way to tell TypeScript that we have type declarations we want it to use for `hello` is to use a triple slash comment like this:

```typescript
/// <reference path="path/to/declarations.d.ts" />

import * as hello from 'hello';

hello.sayHi(new hello.Name("Ivan", "Zlatev"));
```

And have `declarations.d.ts` that looks like this:

```typescript
declare module 'hello' {
    export class Name
    {
        constructor(first : string, last : string);
        first: string;
        last: string;
    }

    export function sayHi(name: Name) : void;
}
```

Notice the use of `declare module 'hello'`. This is an *ambient declaration* and what it means is that we are telling TypeScript: "There exists a module called "hello" which exposes these types".

This is not particlarly great approach and is error and frustration prone - We have to manually maintain the list of references and ensure the ordering is correct.

One benefit of this method is that it allows the creation of `import-all.d.ts` type of file with a list of references - a method used by [typings](https://github.com/typings/typings) (the successor to [tsd](definitelytyped.org)) to create an "index" file with reference list to all installed typings.

## tsconfig.json

The `tsconfig.json` file defines the TypeScript project/workspace when `tsc --project directory_here` is used. Among other things it supports two modes for defining the code files to process in the workspace/project:

* (default) Include all `*.ts` (so also `*.d.ts`) files recursively. Files/paths can optionally be excluded using `exclude: [ "path1", "path2" ]`
* Explicitly include a specific set of files for processing using `include: [ "path1", "path2" ]`

What this means for us is that as long as the our type declarations files are included (implicitly or explicitly) in the `tsconfig.json` **we don't need to use triple slash comments** to reference them, because they will already be part of the project/workspace.

A typical `tsconfig.json` can look like this.

```json
{
  "compilerOptions": {
    "target": "es5",
    "noImplicitAny": true,
    "module": "commonjs",
    "moduleResolution": "node",
    "outDir": "build"
  },
  "compileOnSave": true,
  "exclude": [
    "node_modules",
    ".vscode"
  ]
}
```

You will find that when using [typings](https://github.com/typings/typings) (the successor to [tsd](definitelytyped.org)) it will automatically ensure that the type declarations are included in the paths of `tsconfig.json`.

## Local JavaScript module type declaration

Given a `hello.js` file which exports a module TypeScript will look for  A `hello.d.ts` file (same file name basically).

In our case the type declaration in that file will look like the one below. Note how there is no longer a `declare module 'hello'` bit in the file. We no how an external module type declaration (and not an ambient one) :

```typescript
export class Name
{
    constructor(first : string, last : string);
    first: string;
    last: string;
}

export function sayHi(name: Name) : void;
```


## NPM package "typings:"

Given a module named `hello` TypeScript will look across the NPM packages and inside the `package.json` for any `typings: 'path/to/definitions.ts.d'` and use that. In our case again will look like this:

```typescript
export class Name
{
    constructor(first : string, last : string);
    first: string;
    last: string;
}

export function sayHi(name: Name) : void;
```


## Note the meaning of "ambient" 

In a web browser environment when jQuery is imported - it declares a global variable `$`, which is accessible everywhere. Antother example is the Node environment - we get access to a bunch of functions things without having to import specific packages.

In such a case we are likely to see `*.d.ts` with global ambient declarations ("something has declared X function/variable and it looks like this"). E.g. for jQuery something like [this](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/jquery/jquery.d.ts#L3221):

```typescript
declare var $: JQueryStatic;
```

Unfortunately some of the terminology/naming is inconsistent across tsd, typings and TypeScript itself ([GitHub issue](https://github.com/typings/core/issues/12)), which I personally found frustrating when trying to write type declarations for the first time.






Wallaby.js is an intelligent **test runner for JavaScript** that continuously runs your tests. It reports **code coverage** and other results directly to your code editor **immediately as you change your code**. Wallaby.js uses various tricks to run your tests **as fast as possible**, such as dependency analysis to **only execute tests affected by your code changes** and **parallel test execution**. Wallaby.js is a proprietary commercial software, Visual Studio extension is available for free while in beta.

![wallaby in action](http://wallabyjs.com/assets/img/wallaby.gif)

# Wallaby.js quick links

- [Wallaby.js website](http://wallabyjs.com)
- [Wallaby.js in action video](https://www.youtube.com/watch?v=uUmF16R9JNs)
- [Wallaby.js motivation blog post](http://dm.gl/2015/01/30/wallaby/)
- [Download wallaby.js](http://wallabyjs.com/#download)
- [Getting started with wallaby.js in WebStorm](#getting-started)
- [Getting started with wallaby.js in Visual Studio](http://dm.gl/2015/05/18/wallaby-for-visual-studio/)
- [Testing angular.js application with wallaby.js tutorial](http://dm.gl/2015/02/16/wallaby-angular/)
- [Configuration file](#configuration-file-format)
- [Compilers](#compilers-setting)
- [Preprocessors](#preprocessors-setting)
- [Postprocessor](#postprocessor-setting)
- [Bootstrap](#bootstrap-setting)
- [Node.js and io.js support](#environment-setting)
- [PhantomJs 2 support](#phantomjs-2)
- [Troubleshooting](#troubleshooting)

# Sample projects with wallaby.js configuration

![screen shot 2015-04-14 at 5 59 25 pm](https://cloud.githubusercontent.com/assets/979966/7132813/20047e28-e2d0-11e4-9159-bc57de581695.png)

- [Browser environment](https://github.com/wallabyjs/public/tree/master/sample/jetbrains/)
- [Node.js and io.js environment](https://github.com/wallabyjs/wallaby-node-iojs-sample/)
- [Require.js app](https://github.com/wallabyjs/wallaby-requirejs-sample/)
- [Angular.js app](https://github.com/wallabyjs/wallaby-angular-todomvc-sample/)
- [React.js and JSX](https://github.com/wallabyjs/wallaby-react-todomvc-sample/)
- [ES6 via Babel preprocessor](https://github.com/wallabyjs/wallaby-es6-sample/)
- [ES7 via Babel compiler](https://github.com/wallabyjs/wallaby-babel-es7-sample/)
- [TypeScript support](https://github.com/wallabyjs/wallaby-typescript-sample/)
- [CoffeeScript  support](https://github.com/wallabyjs/wallaby-coffeescript-sample/)
- [Webpack and ES6 modules via Babel](https://github.com/wallabyjs/wallaby-webpack-sample)
- [Browserify](https://github.com/wallabyjs/wallaby-browserify-sample), [Proxyquireify](https://github.com/wallabyjs/wallabify/issues/2)
- [Jspm/SystemJS](https://github.com/wallabyjs/wallaby-jspm-sample)
- [Jest](https://github.com/wallabyjs/wallaby-jest-sample)
- [Sails.js](https://github.com/wallabyjs/sailsjs-mocha-testing-barrels-fixtures-example)
- [LoopbackJs](https://github.com/wallabyjs/public/issues/107#issuecomment-102716117)

# Wallaby.js philosophy

Wallaby.js design is based on few simple principles:

- in most of cases **tests should run automatically** on the code changes, only the change **affected tests** should run, when possible **tests should run in parallel**;
- test and code coverage results should be **displayed in the code editor in realtime**, where and when the code change happens;
- **continuous testing should be software development technique agnostic**; whether you are doing classic TDD/BDD, or tests after, or using some mixed approach or something else - as long as you have tests, running them should be fast and easy.

There are other apps and tools to test your application in different browsers, test it across multiple devices and on different platforms, run your tests in your CI build. Wallaby.js **main focus is to help you writing your code and tests faster**, without unnecessary context switching.

# Supported code editors

- WebStorm **>= 9.0.3**
- PhpStorm **>= 8.0.2**
- IntelliJ IDEA Ultimate **>= 14.0.2**
- RubyMine **>= 7.0.2**
- PyCharm **>= 4.0.4**
- Visual Studio **2013 Update 4**

# Supported testing frameworks

Our plan is to support latest versions of popular testing frameworks as they are released. **If the version of the framework that you are using is not listed, you can still try to run wallaby.js with the closest supported version of the framework supported, it may just work**.

**For browser:**
- jasmine@1.3.1
- jasmine@2.1.3 (default value)
- jasmine@2.2.1
- mocha@2.0.1 (with any expectation framework of your choice)
- mocha@2.2.4
- qunit@1.16.0
- qunit@1.18.0
- [jest@0.4.3](https://github.com/wallabyjs/wallaby-jest-sample)
- [suggest another framework/version](https://github.com/wallabyjs/public/issues/new?title=Support%20...&labels=new%20testing%20framework%2Fversion)

**For node.js and io.js:**
- mocha@2.1.0 (with any expectation framework of your choice)
- [suggest another framework/version](https://github.com/wallabyjs/public/issues/new?title=Support%20...&labels=new%20testing%20framework%2Fversion)

# Configuration file format

Wallaby.js configuration file is a simple JSON or JavaScript file, with a root object containing the properties listed below or exporting a function that returns the object of the same structure.

Like:
```json
{
  "files": [
    "src/*.js"
  ],

  "tests": [
    "test/*Spec.js"
  ]
}
```
or
```javascript
module.exports = function (wallaby) {
  return {
    files: [
      'src/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ]
  };
};
```

`wallaby` parameter contains following properties:

- `localProjectDir` string property returns the project local folder.
- `projectCacheDir` string property returns the project cache folder. Note that wallaby.js uses files from this folder to run tests. It copies files specified in `files` and `tests` lists from `localProjectDir` to `projectCacheDir`.
- `compilers` property allows to access builtin TypeScript, CoffeeScript and Babel [compilers](#compilers-setting).
- `defaults` property allows to set default values for file object properties.

### Files and tests

**`files`** array property specifies an array of source files or file name patterns. The files will be loaded in the test sandbox in the specified order. So if one script loading depends on the other script, the second one should be specified before the first one in the files list. If you have a configuration file for Karma runner or runner HTML page - you can use the order of files from there.

Files may be of any type, not just JavaScript files. While JavaScript files and CSS files are by default included in the internally generated test runner page, other files may be loaded by tests when required (for example HTML files/fragments can be loaded with [jQuery.load](http://api.jquery.com/load/) or any other way you may be using).

**`tests`** array property specifies an array of test files or test file name patterns. Please note that only actual test files should be specified in the tests list (the files with specs/modules and tests); any test helpers or testing frameworks extensions should be specified in the "files" list (normally at the end of the list).

Both files and tests paths are specified relatively to the location of the wallaby.js configuration file.

For example, the configuration sample below makes wallaby.js to track all `.js` files in the `src` folder, and also a CSS and HTML file. The sample also includes all test files from the `test` folder that name ends with `Spec`.

```json
{
  "files": [
    "style/calculator.css",
    "html/calculator.html",
    "src/*.js"
  ],

  "tests": [
    "test/*Spec.js"
  ]
}
```

Files and tests are basically the only mandatory configuration properties, the rest is optional (please note that jasmine v2 is used by default as a testing framework).

Both **`files`** and **`tests`** properties support [glob patterns](https://github.com/isaacs/minimatch), so you can use wildcards to avoid specifying each and every file.

Both **`files`** and **`tests`** property element can either be a string representing file name/name pattern or an object defining additional properties.

### File object

**`pattern`** string property represents the file name or pattern.

**`ignore`** boolean property (default value: **false**) is used to completely exclude the file from being processed by wallaby.js. The setting may be useful to adjust some broader patterns. Note that if you'd like to ignore a file, you may also just add an exclamation mark as a first character of the path to negate the `ignore` property (which is `false` by default). For example, if you have your tests in the same folder with your source files, your config may look like:

```json
{
  "files": [
    "src/*.js",
    "!src/*Spec.js"
  ],

  "tests": [
    "src/*Spec.js"
  ]
}
```

**`instrument`** boolean property (default value: **true**) determines whether the file is instrumented. Setting the property to **false** disables the file code coverage reporting and prevents the file changes to trigger automatic test execution. The setting should normally be set to **false** for libraries/utils/other rarely changing files. Using the setting makes wallaby.js to run your code faster, as it doesn't have to perform unnecessary work.

**`load`** boolean property (default value: **true**) determines whether the file is loaded to the sandbox HTML (via script tag in case of JavaScript files). Setting the property to **false** is useful if you are using some alternative script loading mechanism, such as RequireJs.

In case you would like to override file object properties default values, you may use `wallaby` parameter `defaults` property. For example, if in your configuration files list most of the items should have `instrument` property set to false, you may override the default property value as follows:

```javascript
module.exports = function (wallaby) {
  wallaby.defaults.files.instrument = false;
  return {
    files: [
      'libs/lib1.js',
      'libs/lib2.js',
      'libs/lib3.js',
      'libs/lib4.js',
      { pattern: 'src/*.js', instrument: true }
    ],

    tests: [
      'test/*Spec.js'
    ]
  };
};
```

Note, that the defaults for `files` and `tests` are set separately, so in the example above all tests will be instrumented. To set tests defaults in the example above one could use `wallaby.defaults.tests` object.

To sum it up, the configuration sample below makes wallaby.js to track all `.js` files in the `src` folder and all its subfolders recursively, except those files that names ends with `.generated`, include but avoid instrumenting  libraries in `libs` folder, include but avoid instrumenting test helpers (such as testing frameworks configuration scripts, custom matcher, etc.). The sample also includes all test files from the `test` folder that name ends with `Spec`.

```json
{
  "files": [
    "src/**/*.js",
    { "pattern": "src/**/*.generated.js", "ignore": true },
    { "pattern": "libs/*.js", "instrument": false },
    { "pattern": "test/helpers/*.js", "instrument": false },
  ],

  "tests": [
    "test/*Spec.js"
  ]
}
```
### Environment setting

Wallaby.js supports different environments, such as PhantomJs, node.js and io.js. By default, PhantomJs is used as tests runner environment. Configuration file for node.js or io.js looks like:

```javascript
module.exports = function () {
  return {
    files: [
      'lib/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ],

    env: {
      // use 'node' type to use node.js or io.js
      type: 'node',
      
      // if runner property is not set, then wallaby.js embedded node/io.js version is used
      // you can specifically set the node version by specifying 'node' (or any other command)
      // that resolves your default node version or just specify the path 
      // your node installation, like
      
      // runner: 'node'
      // or
      // runner: 'path to the desired node version'
      
      params: {
        runner: '--harmony --harmony_arrow_functions',
        env: 'PARAM1=true;PARAM2=false'
      }
    }
  };
};
```

**`env.type`** should be set to 'node' to use node.js or io.js or 'browser' to use browser environment (PhantomJs).

**`env.runner`** should be set to your local node/io.js path, or just 'node' (or any other command) that resolves your default node version. If not set, wallaby.js will use its own version of io.js.

**`env.params.runner`** allows to set spawned node process flags.

**`env.params.env`** allows to set spawned node process environment variables.

#### PhantomJs 2

By default, wallaby.js ships with and uses the latest stable PhantomJs version, available for all supported platforms. At the moment, the version is 1.9.8. If you would like to use another PhantomJs, for example PhantomJs 2 available for your platform or any custom build, you may use `env.runner` setting to specify the path to it. For example:

```javascript
module.exports = function () {
  return {
    files: [
      'lib/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ],

    env: {
      type: 'browser',
      runner: '/Users/user/Downloads/phantomjs-2.0.0-macosx/bin/phantomjs'
      // or
      // runner: 'C:\\Tmp\\phantomjs-2.0.0-windows\\bin\\phantomjs.exe'
    }
  };
};
```

Please note, that if you are on **OSX** and have downloaded the **version 2.0** from https://bitbucket.org/ariya/phantomjs/downloads, once you have unzipped it, you will need to run:

```
brew install upx
upx -d path/to/downloaded/unzipped/phantomjs
```

for the reasons [described here](http://stackoverflow.com/questions/28267809/phantomjs-getting-killed-9-for-anything-im-trying).

#### Node process reuse
When running your node.js tests (doesn't apply to browser tests via PhantomJs), wallaby by default tries to re-use once created node.js process(es) to make the runs faster. **If you are relying on the test node process re-start for each run, because your tests are not are not cleaning after themselves**, for example, not closing opened database connections, not stopping started web services, or are registering some callbacks that may be invoked after your test run finishes and interfere with your next test run - you may use [`workers.recycle` setting](#workers-setting) **set to `true`** to make wallaby.js re-start node process for each run.

```javascript
module.exports = function () {
  return {
    files: [
      'lib/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ],

    env: {
      type: 'node'
    },

    workers: {
      recycle: true
    }
  };
};
```

No matter whether you use the setting or not, you will still benefit from wallaby.js incremental test run feature comparing to normal test runner. However, while using the setting allows you to rely on the process re-start and avoid writing any cleanup code, **we recommend to consider adding clean up code and not recycling node workers, because wallaby.js will be able to run your tests even faster** if node processes are reusable.

You can use `afterEach` hooks of your testing framework to clean up after your tests. Another option is to use [`bootstrap`](#bootstrap-setting) function to add some clean-up code that will be executed before each run. In the function you can make sure your database connection is closed, your web server is stopped, required node modules cache is cleaned, and any other conditions are met before running your tests.

### Compilers setting

Wallaby.js [preprocessors](#preprocessors-setting) can transform the content of your files before feeding it to the test runner. But if you would like to have code coverage for some file, wallaby.js has to instrument it before running any preprocessors. It means that wallaby.js has to understand the dialect of the file to be able to parse, change and then emit the file source code. Wallaby.js core supports ES6 (and less) plus JSX. It means that any other dialects or languages that you would like to get code coverage for, need to be compiled to JavaScript before the instrumentation phase.

To allow such compilation from anything to JavaScript (ES6 or less), wallaby.js supports compilers, they run before instrumentation and preprocessors.

Wallaby.js has 3 built-in compilers: **TypeScript, CoffeeScript and Babel**. TypeScript and CoffeeScript compilers are turned on by default, Babel compiler is turned off by default. It means that **you don't even have to configure compiler at all if you are not using Babel compiler and happy with default compiler settings for TypeScript and CoffeeScript**. However, if you specify the `compilers` setting in your config, then you have to list all compilers that you are planning to use, because you are overriding the default value of the `compilers` setting.

**Note that if you are using Babel compiler, you don't need Babel preprocessor anymore.** If you are only using ES6 features and not using any experimental features, such as ES7, you may keep using Babel preprocessor and in this case you don't need Babel compiler. In case of TypeScript and CoffeeScript, compilers is the only way to get coverage for such files with wallaby.js, preprocessors for TypeScript and CoffeeScript are not required.

The format of the compiler setting is almost the same as [preprocessor](#preprocessors-setting) - it is an object with file patterns as keys and compiler functions as values. Here is an example how to configure compilers:

```javascript
var babel = require('babel');

module.exports = function (wallaby) {
  return {
    files: [
      'src/*.js',
      'src/*.ts',
      'src/*.coffee'
    ],

    tests: [
      'test/*Spec.js'
      'test/*Spec.ts',
      'test/*Spec.coffee'
    ],

    compilers: {
      '**/*.js': wallaby.compilers.babel({
        babel: babel,
        // other babel options
        stage: 0    // https://babeljs.io/docs/usage/experimental/
      }),

      '**/*.ts': wallaby.compilers.typeScript({
        // TypeScript compiler specific options
        // https://github.com/Microsoft/TypeScript/blob/master/src/compiler/types.ts#L1584
      }),

      '**/*.coffee': wallaby.compilers.coffeeScript({
        // CoffeeScript compiler specific options
      })
    }
  };
};
```

#### CoffeeScript and TypeScript for node.js

By default wallaby CoffeeScript and TypeScript compiler for node.js renames .coffee/.ts files to .js files, so that in CoffeeScript
``` CoffeeScript
require './app'
```
or in TypeScript
``` TypeScript
require('./app')
```
where original file is app.coffee/app.ts would work.

If you'd like to not do it and use your own renaming strategy, you may pass 'noRename' option to CS compiler:
``` JavaScript
'**/*.coffee': w.compilers.coffeeScript({ noRename: true })
```
or
``` JavaScript
'**/*.ts': w.compilers.typeScript({ noRename: true, module: 1 }) // CommonJs modules
```
and use preprocessors to rename files the way you like:
```
preprocessors: {
  '**/*.coffee': file => file.rename(file.path + '.js').content
}
```
or
```
preprocessors: {
  '**/*.ts': file => file.rename(file.path + '.js').content
}
```

The preprocessor above will change `app.coffee`/`app.ts` to `app.coffee.js`/`app.ts.js` so you may require it as 
``` CoffeeScript
require './app.coffee'
```
or
``` TypeScript
require './app.ts'
```

#### Writing a custom compiler

If you would like to write **your own compiler for some other language** - the process is pretty straightforward. As preprocessor, compiler function can be sync or async, gets `file` argument with content and other properties and supposed to return an object with modified code and a source map.

The important difference between preprocessor and compiler is that a compiler **must** return a source map and it must transform the code to JavaScript. Another mandatory property that a compiler **must** return is `ranges` property, where all coverable ranges of the original file must be listed.

For example, if some file in my hypothetical language looks like

```
var a = b ? c() : d();
```

and conditional operator works in the same manner as in JavaScript (and other languages), then

```
module.exports = function (wallaby) {
  return {
    files: [
      'src/*.hypotheticalLangExt'
    ],

    tests: [
      'test/*Spec.hypotheticalLangExt'
    ],

    compilers: {
      '**/*.hypotheticalLangExt': file => {
        // Parsing file.content

        // Traversing hypothetical language AST and collecting coverable ranges

        // Coverable ranges is an array of locations,
        // where each location is an array of the range
        // start line, start column, end line, end column.

        // In this specific case, we have a variable declaration statement
        // and 2 branches of a conditional expression within it,
        // so ranges would look like:
        // ranges = [
        //   [ 1, 0, 1, 22 ],   // whole statement
        //   [ 1, 12, 1, 15 ],  // c() execution path
        //   [ 1, 18, 1, 21 ]   // d() execution path
        // ];
        // Normally, parsers include (or have an option to include location in AST nodes),
        // so it's just a matter of traversing the right nodes.

        // Generating JavaScript with source map

        return {
          map: sourceMap,
          code: generatedJavaScript,
          ranges: ranges
        };
      }
    }
  };
};
```

### Preprocessors setting

Wallaby.js supports Karma-like preprocessors to transform the content of your files before feeding it to the test runner. **Preprocessors can be both sync or async** (keep reading for more details).

```javascript
module.exports = function () {
  return {
    files: [
      'src/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ],

    preprocessors: {
      '**/*.js': file => require('babel').transform(file.content, {sourceMap: true}),
      '**/*.jsx': file => require('babel').transform(file.content, {sourceMap: true})
    }
  };
};
```

Wallaby preprocessors are extremely simple - you just specify a function or an array of functions to run for each file, that satisfies a given pattern:

```javascript
'**/*.js': file => file.content + '\n// modified once'
```
or
```javascript
'**/*.js': [file => file.content + '\n// modified 1', file => file.content + '\n// modified 2']
```

Wallaby.js uses io.js to run the configuration, so you can use arrow functions, classes, etc. in your wallaby.js configuration, but you don't have to:

```javascript
'**/*.js': function (file) { return file.content + '\n// modified once'; }
```

Although we encourage to use existing npm packages and write simple preprocessors without too much ceremony when possible, the preprocessor functions don't have to be inline, **feel free to encapsulate them anywhere, reuse, wrap and adapt karma plugins or other npm packages**.

Over time we are planning to expose more and more extension points, but would like to do it carefully to keep wallaby.js public API as simple and clean as possible. In a meantime, Karma to Wallaby adapter could be a nice and interesting open source project.

```javascript
var es6ToEs5Preprocessor = file => require('babel').transform(file.content, {sourceMap: true});
var dummyPreprocessor = file => '// dummy prefix\n' + file.content;

module.exports = function () {
  return {
    files: [
      'src/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ],

    preprocessors: {
      '**/*.js': [es6ToEs5Preprocessor, dummyPreprocessor]
      '**/*.jsx': es6ToEs5Preprocessor
    }
  };
};
```

The preprocessor function may take one or two arguments:
```javascript
'**/*.js': file => file.content + '// modified'
```
```javascript
'**/*.js': (file, done) => done(file.content + '// modified')
```
In case of one argument - the preprocessor result is expected immediately, in case of two arguments the preprocessor is async and the second argument is the callback that is invoked when the preprocessor result is ready.

The first argument is a `file` object. 
**`file.content`** property contains current content of the file.
**`file.path`** property contains current content of the file.
**`file.rename(newPath)`** function allows to rename/move the file.

For example, the following preprocessor runs react tools transformation to all `jsx` files and renames them to be `*.jsx.js` files.

```javascript
'**/*.jsx': file => require('react-tools')
                     .transformWithDetails(file.rename(file.path + '.js').content, 
                                           {sourceMap: true})
```

The result of a preprocessor can be:

- a string: modified content of the file,
- an object: with `code` and `map` (or `sourceMap`) properties, where `code` is the file modified content, and `map` is the source map of the transformation.
- an error: error object of `Error` type (passed to `done` callback or thrown from sync preprocessor).

Please note, that if the transformation produces a JavaScript file that **if you are interested to see the coverage for, then returning the source map is essential**. Otherwise, the reported coverage and error stacks may not be correct.

### Postprocessor setting

For every file change or batch of file changes, wallaby.js uses following processing pipeline:

- [compilers](#compilers-setting)
- instrumentation
- [preprocessors](#preprocessors-setting)
- postprocessor
- test run

Wallaby.js [compilers](#compilers-setting) and [preprocessors](#preprocessors-setting) are simple and stateless functions that take file argument and are supposed to returned processed content along with some other related data. It is not recommended to keep any state between function calls because preprocessors as well as compilers may run in different node processes.

Postprocessor is a function that runs for every batch of file changes after all compilers and preprocessors. It has access not to just one file, but to all (compiled and preprocessed, as well as original) files. It is also guaranteed to always run in the same node process, so it can keep some required state.

Considering all above, postprocessor is an ideal place for something like a module bundler, or anything else that needs to keep some state between test runs and have access to all files at the same time.

From the implementation prospective, postprocessor is a function that takes a single argument and is supposed to return a promise.

```javascript
 module.exports = function () {
   return {
     files: [
       'src/*.js'
     ],

     tests: [
       'test/*Spec.js'
     ],

     postprocessor: function (wallaby) {
       return new Promise(
         function (resolve, reject) {
            try {
              // postprocessor work here
              // ...

              // resolve when the work is done
              // or reject on errors
              resolve();

            } catch (err) {
              reject(err);
            }
          });
     }
   };
 };
```

The function argument is wallaby.js context where you can find some properties and functions to control emitted test files. Here is a list of the argument properties and functions:

- `logger` property, allows to log debugging information or errors. Use `logger.debug()` to log any debugging information that will only be reported with `debug: true` setting, or `logger.error`/`logger.warn` to report errors.
- `anyFilesAdded` and `anyFilesDeleted` properties, return whether or not any files have been added or deleted for the current batch of changes.
- `nodeModulesDir` property returns local project node modules folder.
- `baseDir` property returns wallaby cache project folder, please note that tests run on this folder files (not on the local project files).
- `allFiles`, `affectedFiles` and `affectedTestFiles` properties return an array of all file objects that are tracked by wallaby.js, all source and test files affected by the current batch of file changes and just test files affected by the current batch of file changes respectively. `allFiles` set contains `affectedFiles` set, `affectedFiles` set contain `affectedTestFiles` set. 'Affected' in the context of test files means not only changed test files, but also files containing tests to run as a result of the whole change. For example, if file `a.js` is covered by tests from file `b.js` and file `a.js` changes, then `allFiles` will have `a.js`, `b.js` and other project files, `affectedFiles` will contain `a.js` and `b.js`, `affectedTestFiles` will contain `b.js`.
- `createFile` function can create a new file, overwrite or clone/transform an existing one.

File objects inside `allFiles`, `affectedFiles` and `affectedTestFiles` have the following structure:

- `path` string property contains relative path of the file (relative to `baseDir`).
- `fullPath` string property contains full path of the file in the `baseDir`.
- `binary` boolean property returns true if the file is a binary file (file content in this case is base64 encoded string).
- `ts` property contains the file change timestamp.
- `id` property contains the file integer identifier in wallaby.js.
- `type` property contains the file type, like 'js' or 'html'. The type is initially taken from the file name extension. Note that compilers and preprocessors may change the type (compilers do it without changing the file name extension).
- `load` boolean property is the same as in configuration `files` object. In case of JavaScript file type, it allows to determine whether the file is loaded via script tag into the generated sandbox.
- `instrument` boolean property is the same as in configuration `files` object.
- `test` boolean property allows to determine whether the file is a test file.
- `getContent` function returns a promise, when resolved it return the file string content.

File objects that passed as an argument into `createFile` function should have the following structure:

- `path` string property that contains relative path of the file (relative to `baseDir`).
- `load` boolean property is the same as in configuration `files` object. In case of JavaScript file type, it allows to determine whether the file is loaded via script tag into the generated sandbox. If not set, the default value is `true`.
- `order` numeric property that allows to control the file order in the list of all files included into the sandbox. Setting it to some negative value allows to tell wallaby.js that the file should be the first, setting it to `Infinity` will make it the last one.
- `ts` numeric property that allows to set the file timestamp. File timestamps are used in caching mechanism, if the timestamp doesn't change, browser/phantomJs may use a cached copy. If not passed explicitly, `ts` will be set to the current time.
- `content` property is a string content of the file.
- `sourceMap` property allows to pass the source map in case if the created file is a transformed version of some other file. The property should be used in conjunction with `original` file property.
- `original` file property allows to pass another file object, for example from `allFiles`, `affectedFiles` or `affectedTestFiles`. When `original` file is passed along with `sourceMap`, wallaby.js will make sure to correctly connect transformed and original files so that when the transformed file code is executed in the generated sandbox, error stack and code coverage is reported for the original file. It can be useful when you would like to execute transformed version of some file without overwriting the original version.

For examples on how to write your own postprocessor, you may have look into wallaby.js postprocessor for [webpack](https://github.com/jeffling/wallaby-webpack/blob/master/index.js#L56) or [browserify](https://github.com/wallabyjs/wallabify/blob/master/index.js#L55).

### Middleware setting

Wallaby.js copies all files specified in `files` and `tests` lists to its own cache. It instruments these files, runs compilers, preprocessors and postprocessor, and serves all files from the cache. Required files are served to the sandbox from wallaby.js web server.

However, sometimes you may want to just specify some file(s) or a folder that wallaby.js will serve as is, without copying it to its local cache, instrumenting and processing. You may also configure other options of how these files are served, such as cookies, various HTTP headers and so on.

You may serve anything by specifying `middleware` function that takes 2 arguments: [express application instance and express instance](http://expressjs.com/guide/using-middleware.html), and allows to set and use any middleware.

 ```javascript
 module.exports = function () {
   return {
     files: [
       'src/*.js'
     ],

     tests: [
       'test/*Spec.js'
     ],

     // telling wallaby to serve jspm_packages project folder
     // as is from wallaby web server
     middleware: (app, express) => {
       app.use('/jspm_packages',
               express.static(
                  require('path').join(__dirname, 'jspm_packages')));
     }
   };
 };
```

### Bootstrap setting

In case if you have to configure your testing framework or sandbox environment just before a test run starts, you can use the `bootstrap` setting. It allows you to specify a function to be executed just before a test run starts.

 ```javascript
 module.exports = function () {
   return {
     files: [
       'src/*.js'
     ],

     tests: [
       'test/*Spec.js'
     ],

     bootstrap: function (wallaby) {
       // wallaby.testFramework is jasmine/QUnit/mocha object
       wallaby.testFramework.ui('tdd');

       // you can access 'window' object in browser environment,
       // 'global' object or require(...) something in node environment
     }
   };
 };
```

The parameter of the function (named `wallaby` in the code above) has following functions and properties:

- `delayStart()` function that allows to prevent wallaby from starting the test run automatically.
- `start()` function that allows to start the test run manually when required, the function can be used in conjunction with `delayStart()` function to perform some async actions before starting the test run.
- `tests` property that contains the list of all test files that wallaby.js know about.
- `testFramework` property that contains a reference to the test framework object. The property can be used to configure your test framework before executing tests, for example to change some of its default settings.

The API will be extended in future to include more members, feel free to request more wallaby data or control functions to be exposed via the parameter.

**Important**: the `bootstrap` function is executed in the context of the test runner, not in the context of the configuration file.

It means that you can not use objects/variables defined within the configuration file code. For example, this code:

```javascript
module.exports = function () {
  var myParam = 'hello from config';
  return {
    files: [
      'src/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ],

    bootstrap: function (wallaby) {
      console.log(myParam);
    }
  };
};
```

will output `undefined`, because `myParam` will not be defined in the context of the test runner, when the `bootstrap` function will be executed.

However, because the `bootstrap` function is executed in the context of the test runner, you can use its environment to initialize it the way you need.

For example, you can access `window` object if you are running browser tests or use `global` object in the case of node.js tests (or `require` modules you need).

The configuration for a typical scenario of configuring a testing framework, for example `mocha` options, before the test run may look like:

```javascript
module.exports = function () {
  return {
    files: [
      'src/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ],

    bootstrap: function (wallaby) {
      var mocha = wallaby.testFramework;
      mocha.ui('tdd');
      // etc.
    }
  };
};
```

### Delays setting

**`delays`** object property specifies how much time (in milliseconds) wallaby.js should wait before proceeding to the next stage of the automated test run workflow. It's recommended to skip setting the property and leave it up to wallaby.js to decide what timeouts to use, until you actually encounter a case where you think tuning the delays may help. The property is an object with up to 2 properties, each representing a delay before a certain stage. Depending on how frequently you'd like to run your tests or how powerful your system is, tuning the delays may help to optimize wallaby.js and your system performance.

**`edit`** numeric delay property is a number of milliseconds to wait after last code edit and before letting wallaby.js know about the code change.

**`run`** numeric delay property is a number of milliseconds to wait before letting wallaby.js to automatically run your tests as a result of your code changes.

### Workers setting

**`workers`** object property specifies the degree of parallelism used to run your tests and controls the way how wallaby.js re-uses workers. It's recommended to skip setting the property and leave it up to wallaby.js to decide how many processes to use based on your system capacity, until you actually encounter a case where you think tuning the workers count may help.

**`initial`** numeric property specifies the number of parallel processes to run your tests when your start/restart wallaby.js.

**`regular`** numeric property specifies the number of parallel processes to run your tests every time when your code changes.

**`recycle`** boolean property determines whether wallaby.js should restart workers (when set to `true` wallaby.js will restart processes for each run, when set to `false` wallaby.js will use once started processes forever relying on your tests cleanup code). It's only applicable in node.js/io.js environment, because in the browser wallaby.js reloads page anyway.

For example, the configuration sample below makes wallaby.js to wait a half of a second after editing a source file and before registering the code change, and to wait 150 milliseconds before running affected tests after the code change was registered. The configuration also makes wallaby.js to run all tests in 6 separate workers initially, then use up to 2 workers to run tests on your code changes.

```json
{
  "files": [
    "style/calculator.css",
    "html/calculator.html",
    { "pattern": "src/libs/*.js", "instrument": false },
    "src/*.js",
  ],

  "tests": [
    "test/*Spec.js"
  ],

  "delays": {
    "edit": 500,
    "run": 150
  },

  "workers": {
    "initial": 6,
    "regular": 2
  }
}
```

If your tests depend on each other in some way and you would not like to or cannot make them isolated, you may set `initial` and `regular` properties to 1, this will make wallaby to run all of your tests sequentially like in Karma.

### Testing framework setting

**`testFramework`** string property specifies the name and the version of the testing framework that you are using for your tests. It uses following format: "name@version". Please note that because in order to properly support a testing framework, wallaby.js has to integrate with it and extend it, you can not use an arbitrary version of any framework with wallaby.js and may only use those that are supported. Currently supported frameworks/versions are [listed here](#supported-testing-frameworks).

### Debug setting

**`debug`** boolean property, when is set to **true**, produces a detailed wallaby.js log. It can be used to investigate any possible issues and can be supplied when [reporting bugs](https://github.com/wallabyjs/public/issues).

# Under the hood

Wallaby.js consists of two major parts: code editor integration and a local background server. Code editor part is written in the language used by the editor itself, the server part is written in JavaScript and is using io.js.

Whenever a tracked source file changes, code editor sends the diff to the server. The local server has a cache of all the source files, it applies the diff and runs the affected tests.

Wallaby.js server tracks dependencies between your files and tests using a mix of common sense and some dark magic. On a serious note, it's a combination of runtime analysis powered by the code instrumentation and lots of tricky heuristics.

Wallaby.js server uses its snapshot of all the source files to run tests. It's important thing to remember, when you are writing wallaby.js plugin or tests that needs to access file system. The exception is `node_modules` folder, wallaby.js doesn't copy it to its cache (unless you have specifically referencing some of the `node_modules` files in your `files` list), but just uses your project local version.

We are planning to cover more details in a series of blog posts, in a meantime feel free to ask your questions [via email](mailto:wallabyjs@gmail.com).

# Troubleshooting

Wallaby.js issues are registered [using this repository](https://github.com/wallabyjs/public/issues/). Whenever something goes wrong, please report an issue and we will address it as soon as we can.

Unless it's not something obvious and easily reproducible, please make sure to do whatever you can from the checklist:

- If you are running tests in node.js and getting `Cannot find module` error or other 'file not found' type of issues, please make sure that files that you are trying to load are listed in the `files` list. Wallaby.js uses its own cache, so it has to copy used files there. Note, that this doesn't apply to `node_modules` folder, wallaby.js doesn't cache it and uses your local version of it. So no need to list node modules in the `files` list for your node tests (but may be required it for browser tests if you are using node modules as libs).
- If you can, please provide a small sample project, where the error can be reproduced. Best if you create a repository on GitHub and give us a link to your repo. We will have a look, fix wallaby.js issue or the setup issue and send you a pull request, [like this one](https://github.com/Gregoor/wallaby-test/pull/1).
- If you are on Windows and getting `phantom stderr: 'C:\Users\...' is not recognized as an internal or external command, operable program or batch file.` error, download PhantomJs, place it in some folder with the path that doesn't contain spaces and [configure `env.runner` like described here](https://github.com/wallabyjs/public/issues/139).
- If your tests run fine in Karma, but you are having some issues when running them in wallaby.js, try setting [`workers`](#workers-setting) count to 1. If it starts working after the change, it most likely means that your tests depend on each other in some way. For example, one of your test suites that is running first, sets some state, and test suites running later depend on it. You may keep worker number set to 1 to avoid the issue, but we recommend to make your test files isolated, so that wallaby.js can run them in parallel even faster.
- If you have some transient issues in your node.js tests, try using [`workers.recycle` setting](#workers-setting) and **setting it to `true`**. If it helps, then the most likely reason of the issues is that one or more of your tests set some state that causes tests to fail in the next run, because by default wallaby.js is trying to reuse node processes. You may either try adding some clean-up code to make node processes fully reusable as [described here](#node-process-reuse) and make your test runs even faster, or keep using the `workers.recycle` setting set to `true` and rely on the process re-start.
- Clear the local server file cache: currently you can force it to be cleared by just stopping your wallaby run configuration, changing wallaby configuration file content (even just adding a space somewhere inside) and running wallaby run configuration again: if the issue doesn't disappear, keep going through the checklist.
- Review and attach idea.log: the location is available from the "Help - Show Log" menu of your code editor.
- Review and attach wallaby.js log: you can record it while reproducing the issue, by setting the "debug" property to "true" in your wallaby configuration file and copying the log text from the "Wallaby Console" tab of the running wallaby.js tool window.
- When doing some advanced wallaby.js configuration, its local cache may be a useful place to have a look into for troubleshooting. Wallaby.js local cache folder is printed at the very beginning of the wallaby.js log when it starts. After wallaby.js starts, have a look into the `instrumented` subfolder to make sure all of the files are there and in the state you are expecting them to be.

```javascript
module.exports = function () {
  return {
    files: [
      'src/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ],

    debug: true
  };
};
```


# Getting started

*with wallaby.js in WebStorm, IntelliJ IDEA Ultimate, PhpStorm, RubyMine, PyCharm Professional*

*You may also find a sample calculator project with created run configurations and config files for Mocha, QUnit, Jasmine and [in this repository](https://github.com/wallabyjs/public/tree/master/sample/jetbrains).*

After you have downloaded wallaby.js plugin zip file, you can install the plugin by opening Settings - Plugins, clicking "Install plugin from disk" button and selecting the downloaded ZIP file or the unzippbed JAR file if your browser automatically unzips the archive for you. From now on, you will be able to automatically update the plugin right from your IDE.


![plugin](https://cloud.githubusercontent.com/assets/979966/5896520/819ac25e-a582-11e4-8217-e98d3bf2af56.png)

### Configuration file

After the plugin is installed, you can start by creating wallaby configuration file. The configuration file is just a [simple JSON or JavaScript file](#configuration-file-format) in your project root with just a couple of mandatory settings. It's recommended to prefix or suffix it with "wallaby", for example "wallaby.json" or "wallaby.js".

Simple configuration file should include "files", "tests" (and "testFramework", if you are not using jasmine) properties.

```json
{
  "files": [
    "src/*.js"
  ],

  "tests": [
    "test/*Spec.js"
  ]
}
```
or
```javascript
module.exports = function () {
  return {
    files: [
      'src/*.js'
    ],

    tests: [
      'test/*Spec.js'
    ]
  };
};
```

"Files" property should list your source files, "tests" property should list your test files. Any test helpers or other test related files, that are not the files with actual tests/specs, should be listed under the files property. Glob patterns can also be used, just make sure your files are in the order that you would like them to be loaded into the (internally generated) test page.

Finally the "testFramework" property defines the name and the version of the testing framework you will be (or are) using to write your tests. Please note, that you don't need to have it locally installed - wallaby.js is shipped with the versions of the frameworks it supports. The supported testing frameworks [are listed here](#supported-testing-frameworks).

The configuration file example above assumes that your source files are in the "src" folder, that you are using Jasmine and your tests are in the "test" folder and have their names ending with "Spec".

You can read more about the configuration file format and other settings in [the corresponding section](#configuration-file-format).

### Run configuration

Continue with creating a new wallaby.js **Run Configuration**. Select **Edit configurations** from the drop-down menu at the top-right corner of the navigation bar, or from the **Run** menu.

In the configuration you can specify:

- a name for the newly created configuration,
- a path to your wallaby.js [configuration file](#configuration-file-format).


![rc](https://cloud.githubusercontent.com/assets/979966/5896460/a710f81a-a581-11e4-8f7f-015b60b55e9f.png)


Once you have created and saved the configuration, you can start it. First time you run it, wallaby.js may take a minute or two to install all the dependencies it needs, such as node.js and PhantomJs. It only does this once and then uses the saved dependencies. The dependencies are updated automatically when required.

Once you have started the run configuration, in the bottom right corner of the screen you'll see wallaby status indicator. It's job is pretty simple: when it displays a spinner - your tests are running, when it's red - you have some failing tests, when it's green - all of your tests are passing.

### Tool window

When wallaby.js starts, it also displays a tool window. While writing code, you may not need the window to be opened, so you can collapse it. You ca expand it any time by clicking wallaby.js status indicator (or using standard way to open a run configuration tool window).


![status](https://cloud.githubusercontent.com/assets/979966/5896676/24b4b934-a585-11e4-96e1-41db627cf850.png)


In the tool window you can see the "Failing Tests" tab and the "Wallaby Console" tab.

"Failing Tests" tab displays all tests that are currently failing along with the error stacks and registered console.log calls. Some bits of the displayed information are hyperlinks that you can use with your mouse or keyboard to navigate to different places, for example to the exact error line, or a failing test, or a place where something is logged to console.

"Wallaby Console" tab displays some additional information about test execution progress. The tab can also be useful for troubleshooting wallaby.js issues.

### Code coverage

Once wallaby.js is running, you can see the code coverage in the source files that it tracks (specified in the configuration file). The coverage is live, so you can start changing your code and the coverage will automatically be updated, just as you type.

As you can see, there are various colored squares displayed for each line of your source code.


![coverage](https://cloud.githubusercontent.com/assets/979966/5896430/228025e4-a581-11e4-9bb2-eb3fd47c842b.png)


- Gray square means that the source line is not covered by any of your tests.
- Green square means that the source line is covered by at least one of your tests.
- Yellow square means that the source line is only partially covered by some of your tests.
- Red square means that the source line is the source of an error or failed expectation, or is in the stack of an error.
- Pink square means that the source line is on the execution path of a failing test.

Clicking a square against a source code line will display a window with the information about the tests covering the line, including any error messages and console.log messages.

You can search inside the window by using standard **Cmd/Ctrl + F** command and navigate links by using **Cmd/Ctrl + B**.

![window](https://cloud.githubusercontent.com/assets/979966/5896734/3b720036-a586-11e4-9a14-31f6fbdec3e8.png)


Short error messages, expectation failures and console.log messages are also displayed inline, right where and when they happen. It's very convenient, as allows you to avoid context switching and allows to fix the issues in place, as you write your code.

### Context actions

Another way to access some source code line/context actions is to use the light bulb quick actions or just pressing "Alt + Enter" on any line. The actions that you can use include:


![context](https://cloud.githubusercontent.com/assets/979966/5896429/2277c6b0-a581-11e4-9e68-35545d823116.png)


- **Show line test(s) action**: works similar to clicking a source code line square and displays the line related test data.
- **Jump to failing test action**: allows to quickly navigate to the failing test from any 'pink' context (the failing test execution path).
- **Jump to error source action**: allows to quickly navigate to the source of the test error from any 'pink' context (failing test execution path).
- **Show line uncovered regions**: the action is useful for "yellow" lines to display what is exactly not covered in the line. Not covered expressions will be highlighted in red.
- **Show file uncovered regions**: the action is similar to the previous one, it displays uncovered regions for the whole file. The red highlights for both actions can be removed by pressing "Esc" (or you can just start changing the file code and they will disappear).
- **Run scope test(s)**: the action is pretty simple (yet so much wanted by so many people), it just runs a single test (if invoked from within the test) or all related tests (if invoked within some source code covered by the tests).
- **Run file test(s)**: the action runs all tests within the test file it is invoked in, or all tests in all the test files that cover the source file that the action is invoked within.
- **Show last run screen shot**: the action just does what it says. After every test run, wallaby.js captures a screen shot of the generated test runner page, so if you have some tests that are displaying anything - the action could be useful for you. Very powerful combo is using the action in conjunction with the **Run scope test(s)** - you run a specific test, then see the screen shot for the test. Please note that you may often have some "tear down" code, that clears the DOM before or after each test - wallaby.js is smart enough to capture the screenshot before the tear down code.

All of the described actions can also be mapped to any keyboard shortcuts you like to make you even more productive. You can do it in Settings - Keymap (search for wallaby in the keymap settings).


![keymap](https://cloud.githubusercontent.com/assets/979966/5896431/22893332-a581-11e4-8ee7-477ef3e6269c.png)


The recommended workflow is just to keep wallaby.js up and running as you edit your code. It should pick up existing file changes, file deletion, adding new files and tests (as long as they are listed in files patterns in the configuration file). Wallaby.js does its best to track the dependencies between your files and tests, but occasionally you may need to restart it, if your changes are not picked up. You can stop/start wallaby.js any time by stopping/starting the active run configuration.

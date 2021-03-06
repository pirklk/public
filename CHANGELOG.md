<a name="Core 1.0.55, IntelliJ 1.0.18, VS 1.0.4"></a>
# Core 1.0.55, IntelliJ 1.0.18, Visual Studio 2013 1.0.4 (2015-06-01)

## Changes and Bug Fixes

- PhantomJs 2 support [#137](https://github.com/wallabyjs/public/issues/137).
- Made "Jump to Failing Test" action available for any context (not just coverable line) and has made context actions icon color be reflect the latest test results.
- Fixed wallaby.js ReSharper 9.1.1 compatibility issues [#134](https://github.com/wallabyjs/public/issues/134).
- Improved the IntelliJ plugin installation uer experience [#129](https://github.com/wallabyjs/public/issues/129).
- Fixed timing issue on Windows when node process is requested while still connecting to it.
- Signalling live event even if the file cache is invalid but up-to-date (fixes occasional issues when starting to wallaby.js with invalid JavaScript code).

<a name="Core 1.0.52, IntelliJ 1.0.16, VS 1.0.1"></a>
# Core 1.0.52, IntelliJ 1.0.16, Visual Studio 2013 1.0.1 (2015-05-25)

## Changes and Bug Fixes

- Fixed a few Facebook Jest integration issues [#112](https://github.com/wallabyjs/public/issues/112), [#115](https://github.com/wallabyjs/public/issues/115), [#118](https://github.com/wallabyjs/public/issues/118), [#121](https://github.com/wallabyjs/public/issues/121).
- Implemented ES6 assignment property support [#122](https://github.com/wallabyjs/public/issues/122).
- Allowed to reference node modules by relative path from `bootstrap` function for node.js tests [#123](https://github.com/wallabyjs/public/issues/123).
- Added [PhantomJs params passing support](https://github.com/wallabyjs/wallaby-external-script/blob/master/wallaby.js#L15).
- Extended trial session time.

<a name="Core 1.0.45, VS 1.0.0"></a>
# Core 1.0.45, Visual Studio 2013 1.0.0 (2015-05-18)

## Changes and Bug Fixes

- [Visual Studio preview support](http://dm.gl/2015/05/18/wallaby-for-visual-studio/).
- Added projectCacheDir to bootstrap argument [#107](https://github.com/wallabyjs/public/issues/107).
- Fixed PhantomJs page reloading caused memory leak [#108](https://github.com/wallabyjs/public/issues/108).
- Fixed NODE_PATH overriding issue in Jest initializer [#110](https://github.com/wallabyjs/public/issues/110).

<a name="Core 1.0.42, IntelliJ 1.0.14"></a>
# Core 1.0.42, IntelliJ 1.0.14 (2015-05-15)

## Changes and Bug Fixes

- Implemented Facebook Jest support [#44](https://github.com/wallabyjs/public/issues/44).
- Improved initial file scanning performance.
- Added possible infinite loop in test warning.
- Added wallaby core version to the Wallaby Console output.
- Fixed affected tests run when a source file is deleted [#103](https://github.com/wallabyjs/public/issues/103).
- Patched Jasmine spy to create an error with correct stack [#104](https://github.com/wallabyjs/public/issues/104).

<a name="Core 1.0.39, IntelliJ 1.0.12"></a>
# Core 1.0.39, IntelliJ 1.0.12 (2015-05-08)

## Changes and Bug Fixes

- Upgraded TypeScript compiler to 1.5 beta.
- Added licensing mechanism.
- Removed checking it function arguments from Jasmine integration [#96](https://github.com/wallabyjs/public/issues/96).
- Avoid stopping running session for errors with no stack, added chai specific default node setting [#92](https://github.com/wallabyjs/public/issues/92).
- Added a queue of file change commands to avoid sending the file change operations from the core is live.

<a name="Core 1.0.36, IntelliJ 1.0.9"></a>
# Core 1.0.36, IntelliJ 1.0.9 (2015-04-28)

## Changes and Bug Fixes

- Implemented a way to [negate file path](https://github.com/wallabyjs/public#file-object) (inverts ignore property) [#62](https://github.com/wallabyjs/public/issues/62).
- Implemented a way for [overriding defaults for files and tests list pattern objects](https://github.com/wallabyjs/public#file-object) [#71](https://github.com/wallabyjs/public/issues/71).
- Fixed JSX whitespaces instrumentation [#88](https://github.com/wallabyjs/public/issues/88).
- Added passing test number reporting and number of executed affected tests [#86](https://github.com/wallabyjs/public/issues/86).
- Implemented TypeScript internal modules support via switching to built-in TypeScript postprocessor as opposed to just using the compiler.
- Improved file change management to avoid saving it in IDE on every change [#82](https://github.com/wallabyjs/public/issues/82).
- Fixed incorrect instrumentation of ES6 super-call with spread operator [#83](https://github.com/wallabyjs/public/issues/83).

<a name="Core 1.0.28"></a>
# Core 1.0.28 (2015-04-21)

## Changes and Bug Fixes

- Implemented changing coffee files extension to js (for node.js environment) in CoffeeScript builtin compiler be default.
- Implemented console.log invocation for cases when console.log is replaced with a different implementation, for example a sinon spy which results may be used in tests).
- Implemented correct stack mapping for preprocessor renamed files.
- Fixed an issue with screenshot taking and test cleanup code [#73](https://github.com/wallabyjs/public/issues/73).
- Added a trailing path separator to local project dir property.
- Fixed empty variable declaration instrumenting case when the same variable is declared more than once.
- Fixed ES6 default function parameter instrumenting case.
- Fixed an issue with wallaby.js cache breaking for non JavaScript files [#66](https://github.com/wallabyjs/public/issues/66).

<a name="Core 1.0.22"></a>
# Core 1.0.22 (2015-04-14)

## Changes and Bug Fixes

- Implemented [compilers extension point](https://github.com/wallabyjs/public#compilers-setting).
- Implemented built-in TypeScript, CoffeeScript and Babel compilers [#49](https://github.com/wallabyjs/public/issues/49), [#60](https://github.com/wallabyjs/public/issues/60), [#61](https://github.com/wallabyjs/public/issues/61).
- Added support for Mocha 2.2.4, QUnit 1.18.0
- Fixed an issue with running tests with the same name in Jasmine [#64](https://github.com/wallabyjs/public/issues/64).
- Fixed an issue with running tests with the same name in QUnit [#65](https://github.com/wallabyjs/public/issues/65).

<a name="Core 1.0.18"></a>
# Core 1.0.18 (2015-04-04)

## Changes and Bug Fixes

- Implemented JSX spread attribute support.

<a name="Core 1.0.17"></a>
# Core 1.0.17 (2015-04-01)

## Changes and Bug Fixes

- Implemented SystemJS support.
- Fixed failed pasring for files with shebang [#58](https://github.com/wallabyjs/public/issues/58).
- Exposed `projectCacheDir` property from wallaby config argument.

<a name="Core 1.0.13"></a>
# Core 1.0.13 (2015-03-30)

## Changes and Bug Fixes

- Added config.middleware extension point to allow configuring wallaby hosted web server.
- Fixed ES6 module imports instrumentation [#55](https://github.com/wallabyjs/public/issues/55).
- Implemented rest element support in escodegen fork.

<a name="Core 1.0.11"></a>
# Core 1.0.11 (2015-03-27)

## Bug Fixes

- Fixed require.js app files caching issues on Windows (changing app files was not trigerring tests because files were cached in phantomjs until wallaby.js restart).
 
<a name="Core 1.0.9, IntelliJ 1.0.5"></a>
# Core 1.0.9, IntelliJ 1.0.5 (2015-03-26)

## Changes and Bug Fixes

- Various core changes to support postprocessors, enabling to support module bundlers, such as webpack and browserify ([#27](https://github.com/wallabyjs/public/issues/27), [#9](https://github.com/wallabyjs/public/issues/9))
- Fixed an issue when creating one files while wallaby is running sends multiple requests to the server
- Fixed WebStorm file renaming issue ([#43](https://github.com/wallabyjs/public/issues/43))

<a name="IntelliJ 1.0.4"></a>
# IntelliJ 1.0.4 (2015-03-19)

## Bug Fixes

- Fixed WebStorm 10 EAP compatibility issues, including but not limited to ([#42](https://github.com/wallabyjs/public/issues/42))

<a name="Core 1.0.7"></a>
# Core 1.0.7 (2015-03-11)

## Bug Fixes

- Added warnings displayed when tests list contains files from files lists or no tests found ([#35](https://github.com/wallabyjs/public/issues/35))
- Allowed to use local node modules from bootstrap function ([#40](https://github.com/wallabyjs/public/issues/40))
- Fixed buffers caching for binary files, such as images ([#31](https://github.com/wallabyjs/public/issues/31))

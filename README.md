# Fable compiler errors with NPM dependences

Trying to use <https://github.com/mozilla/pdfjs-dist> via <https://github.com/agentcooper/react-pdf-highlighter> and immediately encountered compilations errors with Fable.

## Steps to recreate bug with new repo

1. dotnet new fable
2. dotnet tool restore
3. npm install pdfjs-dist
4. App.fs, take any dependency from the pdfjs-dist module, e.g.,
```
open Fable.Core.JsInterop
let globalWorkerOptions: unit -> unit = importMember "pdfjs-dist/lib/pdf"
```
5. npm run start
6. Observe multiple compilation errors, failing to handle npm dependencies due to not supporting optional-chaining syntax "?." <https://babeljs.io/docs/en/babel-plugin-proposal-optional-chaining>

## Recreate bug with this repo
1. npm install
2. npm run start

## Compilation errors on ?. syntax
```
ERROR in ./node_modules/pdfjs-dist/lib/display/annotation_layer.js 366:34
Module parse failed: Unexpected token (366:34)
You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
|
|       link[jsName] = () => {
>         this.linkService.eventBus?.dispatch("dispatcheventinsandbox", {
|           source: this,
|           detail: {
 @ ./node_modules/pdfjs-dist/lib/pdf.js 244:24-64
 @ ./src/App.fs.js
```
## Versions
- Fable: F# to JS compiler 3.2.9
- Node v14.15.1
- Windows 10
# Modern JavaScript Explained For Dinosaurs
From medium article by Peter Jang [here](https://medium.com/@peterxjang/modern-javascript-explained-for-dinosaurs-f695e9747b70)

## Using js the old school way
- manually downloading and linking files
- add dependencies above index.js so the global scope has those ready (moment)
- hard to find and download new version of libraries
```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>JavaScript Example</title>
  <script src="moment.min.js"></script>
  <script src="index.js"></script>
</head>
<body>
  <h1>Hello from HTML!</h1>
</body>
</html>
```

## Using a package manager
- bower most popular in 2013
- overtaken by npm around 2015
- yarn has become popular alternative late 2016, but still uses npm under the hood

## Using module bundler (webpack)
- commonJS project started in 2009, nodejs most well-known implementation
  - goal to specify JavaScript outside of the browser
  - modules that allow import/export code across files without resorting to global variables
- browserify used to be most popular in 2011
  - enabled use of node.js style require statements on the frontend
  - enabled npm to become the frontend package manager of choice
- webpack eventually became more widely used in 2015
  - fueled by popularity of React frontend framework, which took full advantage of webpack's various features
```
$ ./node_modules/.bin/webpack index.js bundle.js
```
- this command
  1) starts with index.js
  2) finds require statements and replaces them with the appropriate code
  3) outputs to bundle.js
- webpack can read options from a config file in root directory
```javascript
// webpack.config.js
module.exports = {
  entry: './index.js',
  output: {
    filename: 'bundle.js'
  }
};
```

## Transpiling for language features using build step
- converting the code in one language to code in another similar language
  - like Sass, Less, and Stylus for CSS
- CoffeeScript was a popular js transpilier for a while (released around 2010)
- now most people use babel or TypeScript
  - babel transpiles next generation js to older more compatible JavaScript (ES5)
  - most people use babel because it is most similar to vanilla js

## Using a task runner (npm scripts)
- Grunt was most popular in 2013
- Gulp followed shortly after
- now it is most popular to use scripting capabilities built into npm itself

## Conclusion
- things seem to be settling down, particularly with the adoption of the node ecosystem as a viable way to work with the frontend
- framework cli's help with this process, but you often get to a point where you need to do some extra configuration
  - very critical to understand what each piece does and this article goes over
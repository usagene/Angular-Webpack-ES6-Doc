# Angular-Webpack-ES6-Doc
This document is used to capture the process of adopting ES6 into existing Angular 1.x application.

# Setup Webpack
npm install webpack --save-dev

# Setup Babel
npm install babel-core babel-loader --save-dev

# Migration strategy
1. Pick a system module to convert into ES6
2. Create a module-level export file, call it 'myModule.es6.js', note that ".es6" is part of the file name, it is useful for filtering these files from unit testing, bundling perspectives.
3. Pick an existing ES5 file, say "a.js" and convert it into ES6 module with the same file naming convention, so the new file is "a.es6.js".
4. import that file into myModule.es6.js, then register the export object from a.js into Angular. Example code as following:
    import a from './a.es6';
    angular.module('myModule').factory('a', a);

5. Config myModule.es6.js as entry point in Webpack config file as something below:
    module.exports = {
        entry: {
            myModule: "./ngApp/myModule/myModule.es6.js"
        },
        output: {
            path: './ngApp/dist',
            filename: "[name].js"
        },
        module: {
            loaders: [
                {
                    test: /\.js$/,
                    exclude: /(node_modules|scripts)/,
                    loader: 'babel'
                }
            ]
        }
    };
6. Now run webpack command from the root of the project, and notice file myModule.js is created under "ngApp/dist" folder.
7. Bundle and compile this transpiled file with the rest of ES5 javascript files
8. The application is now runing with code from both ES5 and ES6.

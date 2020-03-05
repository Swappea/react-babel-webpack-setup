# React Webpack Setup

* React development setup without using CRA(create-react-app) CLI or template.

# Steps

1. Create a directory for your web project.
2. Inside the directory, run the following command in cmd
   - **"npm init -y"**
   - this command will create package.json with default values
3. Install React and React DOM
   - **"npm install --save react react-dom"** 
4. Install Webpack
   - **"npm install webpack webpack-cli webpack-dev-server html-webpack-plugin --save-dev"**
   - *webpack-cli* provides commands for running and setting up webpack
   - *webpack-dev-server* is used to quickly develop a application like live reloading
   - *html-webpack-plugin* - simplifies creation of HTML files to server webpack bundles
5. Install Babel
   -  **"npm install --save-dev @babel/core @babel/preset-react @babel/preset-env babel-loader"**
   -  *@babel/core* is the core dependency
   -  *@babel/preset-react* add support to react JSXcode
   -  *@babel/preset-env* add support to ES6 Javascript
   -  *babel-loader* transpiles the react code in JS using Babel and webpack.
6. CSS Loaders
   -  **"npm install --save-dev css-loader style-loader"**
   -  *css-loader* collects CSS from all files and convert it into string.
   -  *style-loader* takes the output string from css-loader and put it into style tag under html head.
7. Create folder structure
   * ![image info](./images/folder_structure.png)

   - **src folder** - will contain the main logic of the app
   - **dist folder** - will container our index html and bundled js
   - **webpack.config.js** - will contain webpack configuration and
   - **.babelrc** - will contain the babel configuration
8. Configuring webpack
   - ```javascript
     //webpack.config.js
     const path = require('path');
     const HtmlWebpackPlugin = require('html-webpack-plugin');
     module.exports = {
        entry: './src/main.js',
        output: {
           path: path.join(__dirname, '/dist'),
           filename: 'bundle.js'
        },
        devServer: {
           port: 8080
        },
        module: {
           rules: [
              {
                 test: /\.(js|jsx)$/,
                 exclude: /node_modules/,
                 loader: 'babel-loader',
              },
              {
                 test: /\.css$/,
                 use: [ 'style-loader', 'css-loader' ]
             }
           ]
        },
        plugins:[
            new HtmlWebpackPlugin({
                 template: './dist/index.html'
            }) 
        ]
     }  
    - **entry** - It is the entry point, webpack will start bundling from here
    - **output** - The bundled file will reside here **"/dist/bundle.js"**
    - **endevServertry** - It defines the weback-dev-server configuration. default port for dev-server is 8080.
    - **Module Rules** - These are rules for transpiling
      - **test** - Regular Expression that tells which files will pass through the loader
      - **exclude** - File that loader should ignore
      - **loader** - Name of the loader that we are using
9. Configuring babel(.babelrc file)
   - ```json
     {
        "presets":["@babel/preset-env", "@babel/preset-react"]
     } 
10. package.json
    - add following 2 scripts  
    - ```json
      "start": "webpack-dev-server --mode development --open --hot",
      "build": "webpack --mode production"
11. Index.html
    - ```html
      <!DOCTYPE html>
      <html lang = "en">
         <head>
            <meta charset = "UTF-8">
            <title>React Web</title>
         </head>
         <body>
            <div id = "root"></div>
            <script src = 'bundle.js'></script>
         </body>
      </html>
    - here **bundle.js** will be our react code and ```<div id="root"/>```will be our root element where our react elements will render
12. Onto fun part - Writing React Code
    1.  **main.js**  
        - ```javascript
          import React from 'react';
          import ReactDOM from 'react-dom';
          import App from './App.js';

          ReactDOM.render(<App />, document.getElementById('root'));
          ```  
        - the ```document.getElementById('root')``` which we used as id of the div in index.html
    2. **App.js**
       - ```javascript
         import React, { Component } from 'react';
         import './app.css';
         class App extends Component{
            render(){
               return(
                  <div>
                     <h1>Hello!!</h1>
                     <h2>Welcome to your First React App..!</h2>
                  </div>
               );
            }
         }
         export default App;
         ``` 
       - Create app.css file with some styles of yours
13. Run App for development mode
    - **"npm start"**
14. Build App for production
    - **"npm run build"**  
# Renton Technical College CSI-244

<br />

<div align="center">  
    <img src="logo.jpg" alt="Logo">
    <h3 align="center">Guided Activity 4</h3>
</div>

This repository is a part of CSI-244 at Renton Technical College.

## Guided Activity 4 - Client Side: Fetch API, Handlebars.js and Webpack

1. Clone this repository to your machine.
2. Open the repository in Visual Studio code and follow the instructions below.

In this Guided Activity, we will be focusing on the Client-Side content
rendering and going in-depth with:

* A popular templating library called Handlebars.js
* Webpack for bundling our project
* Fetch API to receive the posts from the server side.

### Step 1. Setting Up the Project

1.1 **Create a Project Directory:**

* Open your terminal or command prompt.
  * Create a new directory for your project and navigate into it:

     ```powershell
     mkdir posts-client
     cd posts-client
     ```

1.2 **Client side setup:**

* Create a `src` folder for our application source files

    ```powershell
    mkdir src
    ```

* Create a `templates` folder for our Handlebars templates files

    ```powershell
    mkdir templates
    ```

* Create a `webpack.config.js` file for our Webpack configuration

    ```powershell
    new-item webpack.config.js
    ```

* Inside `templates`, create a handlebars file named post.handlebars

    ```powershell
    cd templates
    new-item post.handlebars
    ```

* Inside `src` folder, create a js file named app.js (this will be the entry point of our client side application)

    ```powershell
    cd src
    new-item app.js
    ```

* Inside `src` folder, create an index.html with the following code

    ```powershell
    new-item index.html
    ```

    ```html
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title>Document</title>
        </head>
        <body>
            <div id="posts-container"></div>
        </body>
    </html>
    ```

Please note: that we are not including our app.js file here, it will be automatically included in the build by our Webpack bundler later.

### Step 2: Initialize the Project and Install Webpack and Handlebars

Initialize the Project by command:

```powershell
npm init -y
```

This command will create `package.json` file with all initial information about our application itself, dependencies, development dependencies, scripts and etc.

Now, we need to install Webpack, Webpack CLI, html-webpack-plugin, Handlebars, and handlebars-loader as development dependencies:

```powershell
npm install --save-dev webpack webpack-cli webpack-dev-server html-webpack-plugin handlebars handlebars-loader 
```

* webpack: main package of bundler.
* webpack-cli: command line interface supporting for webpack.
* webpack-dev-server: a Webpack development server in a plugin.
* html-webpack-plugin: plugin that that helps Webpack to insert code in to html files.
* handlebars: our template engine.
* handlebars-loader: plugin that helps Webpack to understand Handlebars syntax.

You can check you `package.json` file now, it should look like this:

```json
{
  "name": "posts-client",
  "version": "1.0.0",
  "description": "",
  "main": "webpack.config.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "handlebars": "^4.7.8",
    "handlebars-loader": "^1.7.3",
    "html-webpack-plugin": "^5.6.0",
    "webpack": "^5.90.3",
    "webpack-cli": "^5.1.4",
    "webpack-dev-server": "^5.0.2"
  }
}
```

As you can see, "devDependencies" contain all our packages now.

### Step 3. Creating the Handlebars Template

Edit `templates/post.handlebars` with your desired HTML structure, as example:

```html
<article>
    <h2>{{title}}</h2>
    <p>{{body}}</p>
</article>
```

This is basic HTML syntax with Handlebars expressions -> the basic unit of a Handlebars template. You can use them alone in a `{{value}}`, pass them to a Handlebars helper, or use them as values in hash arguments. You can read more about Handlebars here: https://handlebarsjs.com/guide/expressions.html#basic-usage

### Step 4. Writing the JavaScript inside app.js

In `src/app.js`, write the code to fetch the posts and use the Handlebars
template:

```javascript
const compiledTemplate = require('../templates/post.handlebars');

async function fetchPosts() {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');
    const posts = await response.json();

    const html = posts.map(post => compiledTemplate(post)).join('');
    document.getElementById('posts-container').innerHTML = html;
}

fetchPosts();
```

* `const compiledTemplate = require('../templates/post.handlebars');` We requiring our template file and assign it to `compiledTemplate` variable.
* `async function fetchPosts() { ... }` our async function, where we using Fetch
  API to get posts from `https://jsonplaceholder.typicode.com/posts` placeholder
  server, then we `await` our response and parse it into json object. Then, we
  using `map` method for each post and joining with our `compiledTemplate`. Finally, we inserting posts inside our document (index.html) using div container with id `posts-container` and method `innerHTML`.
* `fetchPosts();` just calling our function.

### Step 5. Configuring Webpack

Edit `webpack.config.js to set up Webpack with handlebars-loader:

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    mode: 'development',
    entry: './src/app.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
    },
    module: {
        rules: [
            {
                test: /\.handlebars$/,
                loader: 'handlebars-loader',
            },
        ],
    },
    plugins: [
            new HtmlWebpackPlugin({
                template: './src/index.html',
            }),
        ],
    };
```

Let's break our Webpack configuration file into code lines and explain:

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
```

These lines import Node.js modules. `path` is a core Node.js module for handling and transforming file paths. `HtmlWebpackPlugin` is a plugin for Webpack that simplifies the creation of HTML files to serve your bundles.

```javascript
module.exports = {
    // ... configuration options
};
```

This is the standard way to define what the module (in this case, your Webpack configuration file) exports. The exported object contains your Webpack configuration.

```javascript
mode: 'development',
```

This sets the mode for Webpack to 'development'. In this mode, Webpack includes helpful error messages and does not minimize the bundle for easier debugging. The alternative is 'production', which optimizes the build for deployment.

```javascript
entry: './src/app.js',
```

This specifies the entry point file of your application, where Webpack starts its bundling process. In your case, it's `app.js` located in the `src` directory.

```javascript
output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
},
```

This tells Webpack where to output the bundled files. The path option specifies the target directory (here it's `dist`). `__dirname` is a Node.js global variable that gets the current directory's absolute path. `filename` specifies the name of the output file, which is `bundle.js` in this case.

```javascript
module: {
    rules: [
        {
            test: /\.handlebars$/,
            loader: 'handlebars-loader',
        },
    ],
},
```

This section defines rules for how to process different types of modules. Here, it specifies that files ending with `.handlebars` should be processed using the `handlebars-loader`. This loader will handle Handlebars templates.

```javascript
plugins: [
    new HtmlWebpackPlugin({
        template: './src/index.html',
    }),
],
```

Here, you're using the `HtmlWebpackPlugin`. This plugin generates an HTML file, injects the script tags for your Webpack bundles, and writes this file to the `dist` directory. The `template` option specifies the HTML file to use as a template (`./src/index.html` in this case).

### Step 6. Running the Project

To run the project, let's add some scripts commands to our `package.json`:

```json
"scripts": {
    "start": "webpack serve --open",
    "build": "webpack"
}
```

Now, our `package.json` should look something like this:

```json
{
  "name": "posts-client",
  "version": "1.0.0",
  "description": "",
  "main": "webpack.config.js",
  "scripts": {
    "start": "webpack serve --open",
    "build": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "handlebars": "^4.7.8",
    "handlebars-loader": "^1.7.3",
    "html-webpack-plugin": "^5.6.0",
    "webpack": "^5.90.3",
    "webpack-cli": "^5.1.4",
    "webpack-dev-server": "^5.0.2"
  }
}
```

Only one thing left, open your terminal or command line tool and run your project:

```powershell
npm start
```

Your browser will open the project automatically and you should see all posts. Now, if you inspect the page code, you should see something like this:

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
        <script defer src="bundle.js"></script>
    </head>
    <body>
        <div id="posts-container">
            <article>
                <h2>sunt aut facere repellat provident occaecati excepturi optio reprehenderit</h2>
                <p>quia et suscipit suscipit recusandae consequuntur expedita et cum reprehenderit molestiae ut ut quas totam nostrum rerum est autem sunt rem eveniet architecto</p>
            </article>
            ...
            ...
            ...
        </div>
    </body>
</html>
```

* `<script defer src="bundle.js"></script>` Webpack automatically bundled all our js code and insert it inside index.html file.
* Inside our div with `posts-container` id, you can see our posts used our template from `posts.handlebars`.

If you have any questions about this assignment, please reach out to myself or our TA for this course.

# Renton Technical College CSI-244

<br />

<div align="center">  
    <img src="logo.jpg" alt="Logo">
    <h3 align="center">Guided Activity 4</h3>
</div>

This repository is a part of CSI-244 at Renton Technical College.

## Guided Activity 4 - Server Side: Express and EJS + Fetch API

1. Clone this repository to your machine.
2. Open the repository in Visual Studio code and follow the instructions below.

In this Guided Activity, we will be focusing on the Client-Side content
rendering and going in-depth with:

* A server-side templating library called EJS
* Express as server for our project
* Fetch API to receive the posts from the server side.

### Step 1. Setting Up the Project

1.1 **Create a Project Directory:**

* Open your terminal or command prompt.
  * Create a new directory for your project and navigate into it:

    ```powershell
    mkdir posts-client
    cd posts-client
    ```

  * Initialize the Project by command in your root directory:

    ```powershell
    npm init -y
    ```

1.2 **Client side setup:**

* Create a `views` folder for our application EJS views files

    ```powershell
    mkdir views
    ```

* Create a `controllers` folder for our controllers files

    ```powershell
    mkdir controllers
    ```

* Create a file named `server.js`

    ```powershell
    new-item server.js
    ```

### Step 2: Install Express and EJS

Now, we need to install Express and EJS:

```powershell
npm install express ejs
```

This command will create `package.json` file with all initial information about our application itself, dependencies, development dependencies, scripts and etc.

You can check you `package.json` file now, it should look like this:

```json
{
  "name": "posts-client",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "ejs": "^3.1.9",
    "express": "^4.18.2"
  }
}
```

* ejs: our server-side template engine.
* express: our framework for server.

### Step 3. Creating the ejs Templates

Edit `templates/post.ejs` with your desired HTML structure, as example:

```ejs
<article>
    <h2>{{title}}</h2>
    <p>{{body}}</p>
</article>
```

This is basic HTML syntax with ejs expressions -> the basic unit of a ejs template. You can use them alone in a `{{value}}`, pass them to a ejs helper, or use them as values in hash arguments. You can read more about ejs here: https://ejsjs.com/guide/expressions.html#basic-usage

### Step 4. Writing the JavaScript inside app.js

In `src/app.js`, write the code to fetch the posts and use the ejs
template:

```javascript
const compiledTemplate = require('../templates/post.ejs');

async function fetchPosts() {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');
    const posts = await response.json();

    const html = posts.map(post => compiledTemplate(post)).join('');
    document.getElementById('posts-container').innerHTML = html;
}

fetchPosts();
```

* `const compiledTemplate = require('../templates/post.ejs');` We requiring our template file and assign it to `compiledTemplate` variable.
* `async function fetchPosts() { ... }` our async function, where we using Fetch
  API to get posts from `https://jsonplaceholder.typicode.com/posts` placeholder
  server, then we `await` our response and parse it into json object. Then, we
  using `map` method for each post and joining with our `compiledTemplate`. Finally, we inserting posts inside our document (index.html) using div container with id `posts-container` and method `innerHTML`.
* `fetchPosts();` just calling our function.

### Step 5. Configuring Webpack

Edit `webpack.config.js` to set up Webpack with ejs-loader:

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
                test: /\.ejs$/,
                loader: 'ejs-loader',
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
            test: /\.ejs$/,
            loader: 'ejs-loader',
        },
    ],
},
```

This section defines rules for how to process different types of modules. Here, it specifies that files ending with `.ejs` should be processed using the `ejs-loader`. This loader will handle ejs templates.

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
    "ejs": "^4.7.8",
    "ejs-loader": "^1.7.3",
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
* Inside our div with `posts-container` id, you can see our posts used our template from `posts.ejs`.

### Step 7. Add Bootstrap 5 to the Project

Let's add Bootstrap 5 to your Webpack project and apply some styles to your posts.ejs and index.html:

7.1 **Install Bootstrap**

First, you need to install Bootstrap. Open your terminal and run the following command:

```powershell
npm install bootstrap
```

7.2 **Install style-loader and css-loader for Webpack**

These are necessary for Webpack to process CSS files. Run the following command in your terminal:

```powershell
npm install --save-dev style-loader css-loader
```

7.3 **Update Webpack Configuration**

Modify your `webpack.config.js` file to include rules for CSS files. Your `module` section should look like this:

```javascript
module: {
    rules: [
        {
            test: /\.ejs$/,
            loader: 'ejs-loader',
        },
        {
            test: /\.css$/,
            use: ['style-loader', 'css-loader'],
        }
    ],
},
```
7.4 **Import Bootstrap CSS**

To use Bootstrap's styles, you need to import its CSS file into your project. You can do this in your main JavaScript file `app.js`. Open your app.js file and add this line at the top:

```javascript
import 'bootstrap/dist/css/bootstrap.min.css';
```

7.5 **Update ejs Template and HTML**

Now, you can use Bootstrap classes in your posts.ejs and index.html files.

You can add Bootstrap classes for styling inside your `post.ejs`:

```ejs
<article class="card mb-4">
    <div class="card-body">
        <h2 class="card-title">{{title}}</h2>
        <p class="card-text">{{body}}</p>
    </div>
</article>
```

In your `index.html`, you can use Bootstrap's grid system or other components to layout your page. For example:

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
    </head>
    <body>
        <div class="container mt-5">
            <div id="posts-container" class="row">
                <!-- Posts will be rendered here -->
            </div>
        </div>
    </body>
</html>
```

7.6 **Bundle Your Project**

After making these changes, you need to re-bundle your project using Webpack. Run:

```powershell
npm run build
```

Note: folder `dist` will be created in your root directory of the project. `dist` is stands for distribution and the files in it SHOULD NOT be edited or modified.

### Step 8. Article page with own template

Let's make our posts clickable and dynamically replace content on the same page, like in Single Page Applications (SPA).

8.1 **Update Your `post.ejs` Template**

First, you need to update your `post.ejs` to make each post unique.

```ejs
<div class="card mb-4 post-item" data-id="{{id}}" style="cursor: pointer;">
    <div class="card-body">
        <h2 class="card-title">{{title}}</h2>
        <p class="card-text">{{body}}</p>
    </div>
</div>
```

We added data attribute here `data-id="{{id}}"`whose value is the article ID, which we already receive along with the title and body.

8.2 **Create `article.ejs` Template**

Create a new `article.ejs` template inside our `template` folder for displaying a single post:

```ejs
<article class="post-detail">
    <button id="back-btn">Back to Posts</button>
    <h1>{{title}}</h1>
    <p>{{body}}</p>
</article>
```

We add `<button id="back-btn">Back to Posts</button>` button with ID, to use it with EventListener later, since we need to go back from single article page.

8.3 **Update Your `app.js` file**

In your `app.js` file, you'll need to handle the click events to either display a single post or return to the list of posts.

Full listing of the `app.js` file now:

```javascript
import 'bootstrap/dist/css/bootstrap.min.css';

const compiledTemplate = require('../templates/post.ejs');
const compiledArticleTemplate = require('../templates/article.ejs');

async function fetchPosts() {
 const response = await fetch('https://jsonplaceholder.typicode.com/posts');
 const posts = await response.json();

 const html = posts.map((post) => compiledTemplate(post)).join('');
 document.getElementById('posts-container').innerHTML = html;

 // Add click listeners to each post
 document.querySelectorAll('.post-item').forEach((item) => {
  item.addEventListener('click', () => {
   const postId = item.getAttribute('data-id');
   fetchPostById(postId);
  });
 });
}

// We calling this async function when .post-item is clicked
async function fetchPostById(postId) {
 const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`);
 const post = await response.json();

 const html = compiledArticleTemplate(post);
 document.getElementById('posts-container').innerHTML = html;

 document.getElementById('back-btn').addEventListener('click', () => {
  fetchPosts();
 });
}

fetchPosts();
```

Inside our `fetchPosts` we add `forEach` loop for each post with class
`.post-item` and adding Event Listeners with `click` event, then we calling
anonymous arrow function where we getting post ID from our `data-id` attribute
using `getAttribute` method and then calling our new callback function
`fetchPostById` and passing `postId` as argument.

`fetchPostById` is pretty similar to `fetchPosts` except we passing `postId` for
it, using own template for single article and then we adding another Event
Listener for our button `back-btn` with `click` event and calling back our
`fetchPosts();`. Our both function now calling each other depends on event.

### Step 9. Final Run

Command:

```powershell
npm start
```

And your final application should start. Additional styles based on Bootstrap 5 you can add by choice and preferences.

If you have any questions about this assignment, please reach out to myself or our TA for this course.

# Renton Technical College CSI-244
<br />    

<div align="center">  
    <img src="logo.jpg" alt="Logo">
    <h3 align="center">Guided Activity 3</h3>
</div>

This repository is a part of CSI-244 at Renton Technical College.

## Guided Activity 4 - Client Side: AJAX, Fetch, AXIOS Requests
1. Clone this repository to your machine.
2. Open the repository in Visual Studio code and follow the instructions below.

In this Guided Activity we will be focusing on the Client-Side and going in depth with a popular request library called AXIOS.

### Step 1: Set Up Your Project

1. **Create a Project Directory:**
   - Open your terminal or command prompt.
   - Create a new directory for your project and navigate into it:
     ```powershell
     mkdir posts-client
     cd posts-client
     ```
    - Create a package.json which we will use to install axios
         ```powershell
         npm init -y
         npm install axios
         ```

2. **Create our js files:**
   - Create a js file named app.js (this will be the entry point of our client side application)
     ```powershell
     new item app.js
     ```
    - Create a js file named requests.js (this will be where we will make our api requests with axios)
     ```powershell
     new item requests.js
     ```
   - Open `index.html` in a text editor and set up a basic HTML structure:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>JSON Placeholder Client</title>
     </head>
     <body>
         <h1>JSON Placeholder Data</h1>
         <div id="data"></div>
         <script src="script.js"></script>
     </body>
     </html>
     ```

3. **Create JavaScript File:**
   - Create a JavaScript file named `script.js`:
     ```bash
     touch script.js
     ```

### Step 2: Write JavaScript to Fetch Data

In `script.js`, you'll write JavaScript code to fetch data from JSON Placeholder:

1. **Fetch Data from JSON Placeholder:**
   - Use the `fetch` API to make a GET request to JSON Placeholder.
   - For this example, let's fetch posts:
     ```javascript
     fetch('https://jsonplaceholder.typicode.com/posts')
         .then(response => response.json())
         .then(posts => {
             const dataDiv = document.getElementById('data');
             posts.forEach(post => {
                 const postDiv = document.createElement('div');
                 postDiv.innerHTML = `<h3>${post.title}</h3><p>${post.body}</p>`;
                 dataDiv.appendChild(postDiv);
             });
         })
         .catch(error => console.error('Error:', error));
     ```

### Step 3: Open Your Application

- Open `index.html` in a web browser to see the application in action.
- The browser will display a list of posts fetched from JSON Placeholder.

### Additional Notes

- **CORS Policy:** JSON Placeholder should handle CORS, allowing you to make requests from your local file. If you encounter CORS issues with other APIs, you might need to serve your HTML file through a local server or use a proxy.
- **Error Handling:** The `fetch` API example includes basic error handling. Always ensure to handle errors appropriately in real-world applications.
- **Styling:** You can add CSS to `index.html` or link an external stylesheet to improve the appearance of your application.
- **Advanced Functionality:** To expand this project, consider adding more features like fetching different types of data (e.g., users, comments), creating a navigation menu to switch between data types, or adding a form to submit data to the API.

This simple client-side application provides a basic understanding of how to fetch and display data from an external API using vanilla JavaScript and HTML.


Create a new commit with the message Guided Activity 4 Complete and push the changes to GitHub


If you have any questions about this assignment please reach out to myself or our TA for this course.

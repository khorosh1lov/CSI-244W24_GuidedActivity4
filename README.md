# Renton Technical College CSI-244
<br />    

<div align="center">  
    <img src="logo.jpg" alt="Logo">
    <h3 align="center">Guided Activity 3</h3>
</div>

This repository is a part of CSI-244 at Renton Technical College.

## Guided Activity 4 - Client Side: Fetch
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

2. **Client side setup:**
   - Create a js file named app.js (this will be the entry point of our client side application)
     ```powershell
     new item app.js
     ```
    - Create a js file named requests.js (this will be where we will make our api requests with fetch)
         ```powershell
         new item requests.js
         ```
    - Create an index.html with the following code bringing in both requests.js and app.js
         ```html
         <!DOCTYPE html>
            <html lang="en">
              <head>
                <meta charset="UTF-8" />
                <meta name="viewport" content="width=device-width, initial-scale=1.0" />
                <title>Document</title>
              </head>
              <body>
                <script src="requests.js"></script>
                <script src="app.js"></script>
              </body>
            </html>
         ```

3. **Create our re-usable requests library:**
   - We are going to create a re-usable library that can send requests using fetch to any API.
   - Lets create the class and then make our first function. This will be a get function and take in a url as a parameter.
   - Insde of  `requests.js`:
     ```JavaScript
     class Requests{
        //This library will use the fetch API to make requests to the server

        //Get request
        //This is an async function
        async get(url){
            //await the fetch
            const response = await fetch(url);
            //await the response to be converted to json
            const resData = await response.json();
            //return the data
            return resData;
         }
     }
     ```
4. **Using our requests library inside of app.js:**
   - Lets make a request from app.js using our new library.
   - We will create a new instance of Requests and then call the get() method.
   - The get method returns a promise which we will handle with a then() and a catch()
   - Insde of  `app.js`:
     ```JavaScript
        const requests = new Requests();

        //When we call a function that returns a promise, we can use the .then() method to handle the promise
        //The .then() method takes a callback function that will be called when the promise is resolved
        //The .catch() method takes a callback function that will be called when the promise is rejected
        requests
          .get("https://jsonplaceholder.typicode.com/posts/")
          .then((posts) => {
            console.log(posts);
          })
          .catch((err) => {
            console.log(err);
          });
     ```
     - Open up the Index.html in the browser and check the developer console and verify that the posts are logged to the console.

### Step 2: Completing requests.js

In `requests.js`, we have defined a get request. Let's define a post, put, and delete:

1. **Post Request:**
   - After the get() method create a post() method. Make sure that you are inside of the class but outside of the get() method.
   - The post method requires both a url and data.
   - A post request sends data through the body.
   - The fetch() method call takes in a url, but can also take in an object with information about the request.
   - We will used this object to set the request type as well as include the data.
     ```javascript
          //Post request
          //This is an async function. It returns a promise
          async post(url, data) {
            //await the fetch
            //the second parameter of fetch method is an object that defines information
            //about the request. Here we are setting the method to POST and the headers
            //to specify that we are sending JSON data
            //The data is passed to the body of the request
            const response = await fetch(url, {
              method: "POST",
              headers: {
                "Content-type": "application/json",
              },
              body: JSON.stringify(data),
            });
            //await the response to be converted to json
            const resData = await response.json();
            //return the data
            return resData;
          }
     ```
2. **Put Request:**
   - Create a put method. Again, be sure that you are inside of the class but outside of the previous methods we created..
   - A put method is similar to a post in that it requires a url and data
   - A put request sends data through the body.
   - The object passed to the fetch call will define the request method as PUT
   - A put request updates an existing record.
     ```javascript
          //Put request
          //This is an async function. It returns a promise
          async put(url, data) {
            //await the fetch
            const response = await fetch(url, {
              method: "PUT",
              headers: {
                "Content-type": "application/json",
              },
              body: JSON.stringify(data),
            });
            //await the response to be converted to json
            const resData = await response.json();
            //return the data
            return resData;
          }
     ```
3. **Delete Request:**
   - Create a delete method. Again, be sure that you are inside of the class but outside of the previous methods we created.
   - A delete request simply needs a url. That url will include an identifier in the query parameter. The identifier will often be the primary key of the record to be deleted.
   - There is no data being sent in the body.
   - The method is set to DELETE
   - Depending on the API some DELETE requests will return a record and some will not. In this case we are not getting back a record and simply returning resource deleted.
     ```javascript
          //Delete request
          //This is an async function. It returns a promise
          async delete(url) {
            //await the fetch
            const response = await fetch(url, {
              method: "DELETE",
              headers: {
                "Content-type": "application/json",
              },
            });
            //Depending on the API there may not be a response body
            //for a delete request. So we can just return a message
            const resData = await "Resource deleted...";
            //return the data
            return resData;
          }
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

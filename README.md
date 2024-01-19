# Renton Technical College CSI-244
<br />    

<div align="center">  
    <img src="logo.jpg" alt="Logo">
    <h3 align="center">Guided Activity 4</h3>
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
     new-item app.js
     ```
    - Create a js file named requests.js (this will be where we will make our api requests with fetch)
         ```powershell
         new-item requests.js
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
   - Inside of  `requests.js`:
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
   - Inside of  `app.js`:
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
   - We will use this object to set the request type as well as include the data.
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
   - Create a put method. Again, be sure that you are inside of the class but outside of the previous methods we created.
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
### Step 3: Creating a UI to interact with the posts

We now have our requests library ready to be used. Lets create a UI

1. **User Interface:**
   - In index.html replace all of the existing code with the following:
     ```html
        <!DOCTYPE html>
        <html lang="en">
          <head>
            <meta charset="UTF-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title>Posts</title>
            <!-- Bootstrap CSS -->
            <link
              href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"
              rel="stylesheet"
              integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN"
              crossorigin="anonymous"
            />
          </head>
          <body>
            <div class="container">
              <h1>Click on a post to see edit or delete it</h1>
              <!-- Edit a post -->
              <form id="edit-post-form">
                <h2>Edit Post</h2>
                <input type="text" id="editId" disabled />
                <input type="text" id="userId" disabled />
                <div class="form-group">
                  <label for="editTitle">Title</label>
                  <input type="text" class="form-control" id="editTitle" required />
                </div>
                <div class="form-group">
                  <label for="editContent">Content</label>
                  <textarea
                    class="form-control"
                    id="editContent"
                    rows="3"
                    required
                  ></textarea>
                </div>
                <button id="btn-edit-post" type="submit" class="btn btn-primary">
                  Save Changes
                </button>
                <button id="btn-delete-post" class="btn btn-danger">
                  Delete This Post
                </button>
              </form>
              <h1>Posts</h1>
        
              <!-- Display all posts -->
              <div id="posts">
                <!-- Posts will be dynamically added here -->
              </div>
            </div>
        
            <!-- Bootstrap JS -->
            <script
              src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"
              integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL"
              crossorigin="anonymous"
            ></script>
        
            <!-- Custom JavaScript -->
            <script src="requests.js"></script>
            <script src="app.js"></script>
          </body>
        </html>
     ```
     - This is simply a basic user interface where we can display the posts as well as some buttons to edit or delete a post.

### Step 4: edit app.js to add functionality to the UI

Now we need to associate events with our index.html.

1. **app.js:**
   - In app.js replace all of the existing code with the following:
   - Make sure to reference the comments in this code if you are unsure how this code works.
     ```javascript
        const requests = new Requests();
        //object to store the retrieved posts
        let allPosts = [];
        //object to store the selected post
        let selectedPost = null;
        //references for the buttons
        let deleteButton = document.getElementById("btn-delete-post");
        deleteButton.addEventListener("click", deletePost);
        let editButton = document.getElementById("btn-edit-post");
        editButton.addEventListener("click", editPost);
        
        // When the page loads, fetch the posts and display them
        window.addEventListener("load", () => {
          requests
            .get("https://jsonplaceholder.typicode.com/posts/")
            .then((posts) => {
              allPosts = posts;
              console.log(allPosts);
              displayPosts(allPosts);
            })
            .catch((err) => {
              console.log(err);
            });
        });
        
        // Function to display posts in the div with id "posts"
        function displayPosts(posts) {
          const postsDiv = document.getElementById("posts");
          postsDiv.innerHTML = "";
          posts.forEach((post) => {
            const postElement = document.createElement("div");
            postElement.textContent = `Id:${post.id} ${post.title}\n${post.body}`;
            postElement.addEventListener("click", () => {
              setSelectedPost(post.id);
            });
            postsDiv.appendChild(postElement);
          });
        }
        //function which takes an id and sets the selected post to the post with that id
        //this function will also move the information from that post into the form for editing
        function setSelectedPost(postId) {
          //set the selected post to the post that matches postId
          selectedPost = allPosts.find((post) => post.id === postId);
          //log just to make sure
          console.log(selectedPost);
          // Set the values in the Edit Post form
          const editForm = document.getElementById("edit-post-form");
          editForm.elements["editTitle"].value = selectedPost.title;
          editForm.elements["editContent"].value = selectedPost.body;
          editForm.elements["editId"].value = selectedPost.id;
          editForm.elements["userId"].value = selectedPost.userId;
        }
        //function that fires when a post is edited
        function editPost(event) {
          //do not submit the form
          event.preventDefault();
          //make sure a post is selected
          if (selectedPost === null) {
            alert("You must select a post to edit");
            return;
          }
          //get the data from the edit post form
          const editForm = document.getElementById("edit-post-form");
          const editTitle = editForm.elements["editTitle"].value;
          const editContent = editForm.elements["editContent"].value;
          const editId = editForm.elements["editId"].value;
          const userId = editForm.elements["userId"].value;
          //create an object with the updated post data
          const updatedPost = {
            id: editId,
            title: editTitle,
            body: editContent,
            userId: userId,
          };
          //make a put request including the updated post
          requests
            .put(`https://jsonplaceholder.typicode.com/posts/${editId}`, updatedPost)
            .then((response) => {
              console.log(response);
              // Update the selected post with the updated values
              selectedPost.title = editTitle;
              selectedPost.body = editContent;
              //update all posts with the updated post
              allPosts = allPosts.map((post) => {
                if (post.id === selectedPost.id) {
                  return selectedPost;
                } else {
                  return post;
                }
              });
              // Display the updated post
              displayPosts(allPosts);
            })
            .catch((err) => {
              console.log(err);
            });
        }
        //function that fires when a post is deleted
        function deletePost(event) {
          event.preventDefault();
          if (selectedPost === null) {
            alert("You must select a post to delete");
            return;
          }
          const editForm = document.getElementById("edit-post-form");
          const editId = editForm.elements["editId"].value;
          requests
            .delete(`https://jsonplaceholder.typicode.com/posts/${editId}`)
            .then((response) => {
              console.log(response);
              //remove the selected post from all posts
              allPosts = allPosts.filter((post) => post.id !== selectedPost.id);
              // Display the updated post
              displayPosts(allPosts);
            })
            .catch((err) => {
              console.log(err);
            });
        }

     ```
     - This code uses the get(), put() and delete() methods from our requests library.
     - The window load event makes a get request and retrieves all of the posts.
     - The editpost() function makes a put request and passes data from the edit form
     - The deletepost() function makes a delete request and includes the id of the post to be deleted in the url

Create a new commit with the message Guided Activity 4 Complete and push the changes to GitHub


If you have any questions about this assignment please reach out to myself or our TA for this course.

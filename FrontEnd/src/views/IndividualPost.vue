<template>
    <div class="individual-post">
      <h1>{{ "Post " + post.id }}</h1>
      <div class="post-body">
        <textarea>{{ post.body }}</textarea>
      </div>
      <div class="post-actions">
        <button @click="updatePost">Update</button>
        <button @click="deletePost">Delete</button>
      </div>
    </div>
  </template>
    
  <script>
  export default {
    data() {
      return {
        post: {},
      };
    },
    mounted() {
      // Fetch the data for the individual post using the route parameter
      const postId = this.$route.params.id;
      this.fetchPost(postId);
    },
    methods: {
      fetchPost(postId) {
        // Fetch the data for the individual post
        fetch(`http://localhost:3000/post/get/${postId}`)
          .then((response) => response.json())
          .then((data) => {
            this.post = data; // Directly assign the response data
          })
          .catch((error) => {
            console.error("Error fetching individual post:", error);
          });
      },
  
      updatePost() {
        // Get the updated body directly from the textarea
        const updatedBody = document.querySelector(".post-body textarea").value;
  
        // Send a request to update the post's body on the server
        fetch(`http://localhost:3000/post/update/${this.post.id}`, {
          method: "PUT", 
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            body: updatedBody,
          }),
        })
          .then((response) => response.json())
          .then((data) => {
            console.log("Post updated:", data);
  
            // After updating, navigate back to the home page
            this.$router.push("/");
          })
          .catch((error) => {
            console.error("Error updating post:", error);
          });
      },
  
      deletePost() {
        const postId = this.post.id;
  
        // Send a request to delete the post on the server
        fetch(`http://localhost:3000/post/delete/${postId}`, {
          method: "DELETE",
          headers: {
            "Content-Type": "application/json",
          },
        })
          .then((response) => {
            if (response.ok) {
              console.log("Post deleted successfully");
  
              this.$router.push("/");
            } else {
              console.error("Error deleting post:", response.statusText);
            }
          })
          .catch((error) => {
            console.error("Error deleting post:", error);
          });
      },
    },
  };
  </script>
  
  <style>
  .individual-post {
    margin: 0 auto;
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }
  
  .post-body {
    margin: 20px 0;
    width: 60%;
    height: 50%;
  }
  
  .post-actions button {
    border-radius: 25px;
     background-color: #4CAF50; 
     color: white;
     border: none;
     padding: 15px 32px;
     text-align: center;
     text-decoration: none;
     display: inline-block;
     font-size: 16px;
     margin: 4px 2px;
     cursor: pointer;
     transition-duration: 0.4s;
  }
  
  .post-actions {
    display: flex;
    gap: 20px;
  }
  
  textarea {
    width: 100%;
    height: 100%;
    padding: 10px;
    box-sizing: border-box;
    border: 1px solid #ccc;
    border-radius: 8px;
    font-size: 14px;
    resize: vertical; /* Allow vertical resizing */
  }
  </style>
    
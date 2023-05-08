Download Link: https://assignmentchef.com/product/solved-web422-assignment-6
<br>
The purpose of this assignment is to add additional functionality to our ongoing “Blog” project.  This will include the ability to create new posts as well as edit / delete existing posts, allow users to comment on posts and track the number of “views” per post.

Unfortunately, implementing the complete list of features for this blog will take too much time and this is our final assignment.  We will however, end with a ton of functionality in place and it will serve as an excellent starting point if you wish to build out the complete project.  This might include implementing the search functionality,  updating the “Home” page components to show real blog data and/or ensuring users must be logged in before modifying blog data (either directly through the app or indirectly through the API).




<strong>NOTE</strong>: If you are unable to start this assignment because there was an issue with your Assignment 5, please email your professor for a fresh (working) copy.




Specification:

To begin this assignment, you will have to make a copy of your (now complete) assignment 5.  Once this is complete, open up the copied folder in Visual Studio Code and start working from there.




<h1>Step 1:  Creating new PostService Methods</h1>




One of the major features of this assignment is to enable the creation, modification and deletion of blog posts using with a UI within the app.  To facilitate this, we must create the following new methods within our PostService (post.service.ts):




<strong> </strong>

<h2>getAllPosts():Observable&lt;BlogPost[]&gt;</h2>

<strong> </strong>

<ul>

 <li>Fetches all blog posts using the path: YOUR API/api/posts?page=1&amp;perPage=<strong>Number.MAX_SAFE_INTEGER </strong>where “Number.MAX_SAFE_INTEGER” is a constant value of: 2<sup>53</sup> – 1 <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER">(</a><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER">https://developer.mozilla.org/en</a><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER">US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER</a><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER">)</a>.</li>

</ul>




This will ensure that all of the blog posts are returned using a single AJAX request.

<h2>newPost(data: BlogPost): Observable&lt;any&gt;</h2>

This function must invoke the “post” method on the injected “http” service with the data parameter as the body of the request, ie:

return this.http.post&lt;any&gt;(`YOUR API/api/posts`, data);

updatePostById(id: string, data: BlogPost): Observable&lt;any&gt;

This function must invoke the “put” method on the injected “http” service with the data parameter as the body of the request, ie:

return this.http.put&lt;any&gt;(`YOUR API/api/posts/${id}`, data);

<h2>deletePostById(id: string): Observable&lt;any&gt;</h2>

This function must invoke the “delete” method on the injected “http” service with the data parameter as the body of the request, ie:

return this.http.delete&lt;any&gt;(`YOUR API/api/posts/${id}`);

<h1>Step 2:  Creating an “Admin” Section (New Components)</h1>

To help manage our blog posts, we will create a new “admin” section (ie: all routes responsible for adding, editing or removing blog posts will begin with “admin/”.

Before we begin to add our new “admin” components, we must be sure to <strong>add the FormsModule</strong> to the <strong>imports</strong> array in <strong>app.module.ts</strong> (enabling our app to use template-driven forms within the components).




Next, create 3 new components and add them to the routing configuration as follows:




<h2>PostsTableComponent (route: admin)</h2>




This component will be responsible for showing all of the current posts within the blog in a single table, acting as our primary interface to access existing posts.  This means that the component must:




<ul>

 <li>Import &amp; inject the “PostService” service</li>

 <li>Import &amp; inject the “Router” service from “@angular/router”</li>

 <li>Create a property blogPosts of type: Array&lt;BlogPost&gt; with a default value of [] (empty array)</li>

 <li>Set the value of “blogPosts” in the ngOnInit() method using the “getAllBlogPosts()” method of the “PostService”</li>

</ul>

(defined above)




The component’s template must consist of a single table with the fields: “Title” “Post Date” &amp; “Category” as well as a button with the text “+  New Post”.  Each row of the table will be a “blogpost” from the “blogPosts” array.




The following HTML can be used as a starting point:




&lt;br /&gt;

&lt;div class=”container”&gt;

&lt;div class=”row”&gt;

&lt;div class=”col-md-12″&gt;

&lt;a class=”btn btn-success btn-sm pull-right”&gt;+&amp;nbsp;&amp;nbsp;New Post&lt;/a&gt;&lt;br /&gt;&lt;br /&gt;             &lt;table class=”table table-hover”&gt;

&lt;thead&gt;

&lt;tr&gt;

&lt;th&gt;Title&lt;/th&gt;

&lt;th&gt;Post Date&lt;/th&gt;

&lt;th&gt;Category&lt;/th&gt;

&lt;/tr&gt;

&lt;/thead&gt;

&lt;tbody&gt;

&lt;tr&gt;

&lt;td&gt;title&lt;/td&gt;

&lt;td&gt;post Date (formatted using the “date: ‘mediumDate'” pipe&lt;/td&gt;                         &lt;td&gt;post category&lt;/td&gt;

&lt;/tr&gt;

&lt;/tbody&gt;

&lt;/table&gt;

&lt;/div&gt;

&lt;/div&gt;

&lt;/div&gt;

&lt;br /&gt;




With the template in place and rendering the blog data, the rest of the component’s functionality can be implemented, including:




<ul>

 <li>Ensuring that the “+ New Post” links to “/admin/newPost” (using client-side routing)</li>

 <li>Ensuring that when a specific row is clicked, the user will be redirected to the route “admin/post/<strong>id</strong>” where <strong>id</strong> is the “_id” value of the post in the row clicked. This will involve the following steps:

  <ul>

   <li>Writing a click handler function in the component, ie “rowClicked(e, id)” where e is $event, and “id” is the post id of the row clicked. This function will use the “<strong>id</strong>” value with the “router” service to navigate to the path: “admin/post/<strong>id</strong>” using the “navigate” method.</li>

  </ul></li>

</ul>




(see “Linking to Parameterized Routes from the notes here: <a href="https://web422.ca/notes/angular-routing-contd">https://web422.ca/notes/angular</a><a href="https://web422.ca/notes/angular-routing-contd">–</a><a href="https://web422.ca/notes/angular-routing-contd">routing</a><a href="https://web422.ca/notes/angular-routing-contd">contd</a>

<ul>

 <li>Invoking the “rowClicked” event for each &lt;tr&gt; element in the table of posts. This should ensure that the user can navigate to a specific post by clicking on it in the row.</li>

</ul>




<h2>EditPostComponent (route: admin/post/:id)</h2>

<strong> </strong>

This component will show the post information for the post with an _id value of the route parameter “:id” in an editable form with the ability to “remove” the post (<strong>Note</strong>: We will not be editing any of the post “comments” for this assignment).




Editing an exist post will primarily involve editing the “Title”, “Featured Image”, “Category”, “tags” as well as the “Post” itself.  This means that the component must:




<ul>

 <li>Import &amp; inject the “PostService” service</li>

 <li>Import &amp; inject the “Router” and “ActivatedRoute” services from “@angular/router”</li>

 <li>Create a property blogPost of type: BlogPost</li>

 <li>Create a property tags of type string</li>

 <li>Set the value of “blogPost” in the ngOnInit() method using the “getPostById(<strong>id</strong>)” method of the “PostService” (defined above) such that the value of <strong>id</strong> is the value passed in using the route parameter “id” (this value should be something like: “5e5ffe8b0f706590aae79d92”. <strong>HINT:</strong> the “snapshot” method of the “ActivatedRoute” service can be used here <a href="https://web422.ca/notes/angular-routing-contd">(</a><a href="https://web422.ca/notes/angular-routing-contd">https://web422.ca/notes/angular</a><a href="https://web422.ca/notes/angular-routing-contd">–</a><a href="https://web422.ca/notes/angular-routing-contd">routing</a><a href="https://web422.ca/notes/angular-routing-contd">–</a><a href="https://web422.ca/notes/angular-routing-contd">contd</a><a href="https://web422.ca/notes/angular-routing-contd">)</a></li>

 <li>Set the value of the component’s tags property to this.blogPost.tags.toString(); once the blogpost is loaded (previous step). This will allow us to show the tag values as a single string in the UI.</li>

</ul>




Once this is complete, the template must be created.  The following HTML can be used as a starting point (note: all properties can bind directly to the “blogPost” properties, with the exception of “tags”, which much bind to the component’s tags property:




&lt;br /&gt;

&lt;div class=”container”&gt;

&lt;div class=”row”&gt;

&lt;div class=”col-md-12″&gt;

&lt;form&gt;

&lt;div class=”form-group”&gt;

&lt;label&gt;Title&lt;/label&gt;

&lt;input type=”text” class=”form-control”&gt;

&lt;/div&gt;

&lt;div class=”form-group”&gt;

&lt;label&gt;Featured Image (URL)&lt;/label&gt;

&lt;input type=”text” class=”form-control”&gt;

&lt;/div&gt;

&lt;div class=”form-group”&gt;

&lt;label&gt;Post&lt;/label&gt;

&lt;textarea class=”form-control” style=”min-height:200px”&gt;&lt;/textarea&gt;

&lt;/div&gt;

&lt;div class=”form-group”&gt;

&lt;label&gt;Category&lt;/label&gt;

&lt;input type=”text” class=”form-control”&gt;

&lt;/div&gt;

&lt;div class=”form-group”&gt;

&lt;label&gt;Tags&lt;/label&gt;

&lt;textarea class=”form-control”&gt;&lt;/textarea&gt;

&lt;/div&gt;




&lt;div class=”pull-right”&gt;&lt;button class=”btn btn-danger btn-sm”&gt;Delete Post&lt;/button&gt; &amp;nbsp; &lt;button type=”submit” class=”btn btn-success btn-sm”&gt;Update Post&lt;/button&gt;&lt;/div&gt;

&lt;/form&gt;

&lt;/div&gt;

&lt;/div&gt;

&lt;/div&gt;

&lt;br /&gt;




Once this template has been implemented correctly, you should be able to see information for a specific post, ie:










The remaining tasks involve implementing the functionality for the “Update Post” and “Delete Post” buttons.  Let’s start with “Update Post”:




<ul>

 <li>Create a “submit” handler, ie: “formSubmit()” that executes when the form is submitted. It must perform the following tasks:</li>

</ul>




<ul>

 <li>Set the value of blogPost.tags to the value of the “tags” property, converted to an array (ignoring optional whitespace). This can be done using the code:</li>

</ul>




this.tags.split(“,”).map(tag =&gt; tag.trim()); // convert the string to an array and remove whitespace




<ul>

 <li>Invoke the “updatePostById(id)” method of the PostService using the value of blogPost._id and passing the value of blogPost. Once this method has completed, use the “router” service to redirect to the “/admin route using the “navigate” method, ie: navigate([‘admin’]);</li>

</ul>




With “Update Post” complete, let’s move on the “Delete Post” button:




<ul>

 <li>Create a “click” handler, ie “deltePost()” that executes when the “Delete Post” button is clicked. It must perform the following tasks:</li>

</ul>




<ul>

 <li>Invoke the “deletePostId(id)” method of the PostService using the value of blogPost._id. Once this method has completed, use the “router” service to redirect to the “/admin route using the “navigate” method, ie: navigate([‘admin’]);</li>

</ul>




<h2>NewPostComponent (route: admin/newPost)</h2>

<strong> </strong>

The new Post component is very similar to our EditPost component, however we will not be populating the form with existing data and there will not be a “Delete Post” button.




To begin, update the NewPostComponent according to the following specification:




<ul>

 <li>Import &amp; inject the “PostService” service</li>

 <li>Import &amp; inject the “Router” service from “@angular/router”</li>

 <li>Create a new property “blogPost” of type “BlogPost” with a default value of <strong>new BlogPost();</strong></li>

 <li>Create a property tags of type string</li>

 <li>Copy the same HTML template from EditPostComponent (edit-post.component.html) and paste it into the template for this component (new-post.component.html), making sure to:

  <ul>

   <li>Remove the “Delete Post” button o Change the text “Update Post” to “Add Post”</li>

  </ul></li>

 <li>Create a “formSubmit()” method and ensure that it’s executed when the form is submitted. This function must adhere to the following specifications:

  <ul>

   <li>Set the value of blogPost.tags to the value of the “tags” property, converted to an array (ignoring optional whitespace). This can be done using the code:</li>

  </ul></li>

</ul>




this.tags.split(“,”).map(tag =&gt; tag.trim()); // convert the string to an array and remove whitespace o Set the value of blogPost.isPrivate to false o Set the value of blogPost.postDate to new Date().toLocaleDateString(); o Set the value of blogPost.postedBy to “WEB422 Student” o Set the value of blogPost.views to 0

<ul>

 <li>Invoke the “newPost(post)” method of the PostService using the value of blogPost. Once this method has completed, use the “router” service to redirect to the “/admin route using the “navigate” method, ie: navigate([‘admin’]);</li>

</ul>




<h1>Step 2:  Commenting on Posts</h1>

The next missing piece of functionality is the ability for users to comment on specific blog posts.  To enable this functionality, we must work primarily with the PostDataComponent, since it encapsulates the form controls for adding new comments:




<ul>

 <li>Open the post-data.component.html file and locate the &lt;form&gt; element with class “commenting-form”. Here, we must add an ngSubmit event handler that invokes a submit function, ie: “submitComment()” and remove the</li>

</ul>

‘action=”#”‘ property

<ul>

 <li>In the PostDataComponent, create two new fields:

  <ul>

   <li>commentName: string; o commentText: string;</li>

  </ul></li>

 <li>In the post-data.component.html file, locate the &lt;input type=”text”&gt; element with the name “username” and use the template-driven forms binding syntax to sync its value to “commmentName”</li>

 <li>Similarly, locate the &lt;textarea&gt; element with the name “usercomment” and use the template-driven forms binding syntax to sync its value to “commentText”</li>

 <li>In the PostDataComponent add the previously mentioned “submitComment()” function with the following functionality:

  <ul>

   <li>“push” a new object onto the “comments” array for the current post with the properties:

    <ul>

     <li>author: this.commentName</li>

     <li>comment: this.commentText</li>

     <li>date: new Date().toLocaleDateString()</li>

    </ul></li>

   <li>Invoke the “updatePostById(id,data)” method, passing the current post’s _id attribute as the “id” and the current post as the “data” property</li>

   <li>Once the updatePostById() has completed (ie: in the first “subscribe” callback method), reset the values of this.commentName &amp; this.commentText</li>

  </ul></li>

</ul>




<h1>Step 3:  Counting the “Views”</h1>

The last piece of functionality that we need to update before we publish our app, is to track the number of times each post has been viewed.  Currently, each post has a “views” property, but it remains static, despite multiple viewings of the same post.  To fix this, we must edit the “PostDataComponent” according to the following specification:




<ul>

 <li>Notice how, in the ngOnInit() method, we invoke getPostById(). When this is complete, we set the value of this.post to the returned data from our API.  For us to track the number of times this component has been “viewed”, we must add additional code in this callback (subscribe) method.</li>

</ul>

<ul>

 <li>After the code to set the value of this.post (in the subscribe method), increase the value of “views” on this.post by one</li>

 <li>Invoke the “updatePostById(id, data)” method on the injected “PostService” and provide the value of the current post._id and post as its parameters. Since we do not have to execute any code once its complete, we can invoke the “subscribe” method with no paramters, ie:</li>

</ul>




this.postData.updatePostById(this.post._id, this.post).subscribe();




<h1>Step 4:  Publishing the App</h1>

As a final step, we must publish the application online.  This can be any of the methods discussed in class, ie: <strong>Heroku</strong>, <strong>Netflfy</strong> or Seneca’s <strong>Matrix</strong> server.




However, if you choose to publish the application on Netflify, you must use a <strong><u>private</u> Github Repository:  </strong>

<strong> </strong>

<strong> </strong>

<strong> </strong>
# <img src="https://cloud.githubusercontent.com/assets/7833470/10899314/63829980-8188-11e5-8cdd-4ded5bcb6e36.png" height="60"> Angular Auth with Satellizer

<!--9:45 5 minutes -->

<!--Hook: Remember having tons of fun with Passport?  Wish you could do that with a front-end integration?  Well with JWT, Satellizer, and Angular, you can make your authentication footprint smaller, more secure, and more...on the front-end.  Let's get to it! -->

> **Note**: This will be a pair programming lab.  Choose one developer's machine to work on during this lab, but make sure you share your code at the end of the class.

**Objective:** **Implement** Angular authentication with Satellizer. Your goal is to have working sign up and log in functionality.

## What is JWT?
 * <a href="http://jwt.io" target="_blank">JSON Web Tokens</a>

## What is Satellizer?

From the [docs](https://github.com/sahat/satellizer):

>Satellizer is a simple to use, end-to-end, token-based authentication module for AngularJS with built-in support for Google, Facebook, LinkedIn, Twitter, Instagram, GitHub, Bitbucket, Yahoo, Twitch, Microsoft (Windows Live) OAuth providers, as well as Email and Password sign-in.

<!--It is vital that groups not move on until everyone has made it through each major chunk.  Explain what we're doing in each chunk, have a dev explain it at a high level, and make sure folks know to help others if they are done, rather than moving on. Share solutions at end of each major chunk, but not before.-->

<!--9:50 20 minutes -->

## App Setup

1. Fork and clone this repo

2. Once you're in your app directory, run `npm install` to install the required `node_modules`.

3. In one Terminal window, run `mongod`, and in another, run `nodemon`.  You will see an error about not finding a `.env` file.  We'll fix that soon.

4. Navigate to `localhost:9000` in the browser. You should see an empty page and an angry red error message in the Chrome console.

5. **BEFORE WRITING ANY CODE**, open up `models/user.js`, `resources/auth.js`, and `server.js`. Go through these files and look through each code block to see what is already in place for you.

6. Open up `views/index.html` and `public/scripts/app.js`, and see what's in there also.

7. Add a "dot" file, called **`.env`** to the root directory. Add this line to the file:

  ```
  TOKEN_SECRET=yoursupersecrettoken
  ```

  This is the secret your server will use to encode the JWT token for each user.

8. Before hooking up the front-end, test your server routes via Postman:
  * Send a `GET` request to `localhost:9000/api/me` just going to that route in your browser. You should see the message: "Please make sure your request has an Authorization header."
  * Send a `POST` request using Postman to `/auth/signup` with a test `email` and `password`. You should see a token that was generated by the server. (You are simulating a form submission, so make sure to use `x-www-form-urlencoded` and send your data(email and password) in the `body` of the request).
  * Send a `POST` request to `/auth/login` with the `email` and `password` you just used to create a new user. You should again see a token that was generated by the server.

<!--10:10 25 minutes -->

## Satellizer

1. Now it's time to implement authentication from the client. First, you need to include Satellizer in your Angular app:
  * Add the Satellizer CDN to `index.html` (TODO #1).
  ```   <script src="https://cdn.jsdelivr.net/satellizer/0.14.1/satellizer.min.js"></script> ```
  * Add the Satellizer module to your Angular app in `app.js` (TODO #2).  Not sure what to inject?  Check the [docs](https://github.com/sahat/satellizer#usage)
  * Check that you can navigate between your routes (`/`, `/signup`, and `/login`).

2. Follow the instructions in `public/scripts/app.js` to finish implementing `Account.login()` (TODO #3, #4, #5). You can test this on `/login/` with the email and password you used to create a user in setup.


   <details><summary>TODO #3 Hint</summary>
   
     - `console.log(response)` to find the token we're setting.
   </details>
   <!--$auth.setToken(response.data.token)-->

   <details><summary>TODO #4 Hint</summary>
   
    - How would we set `new_user` to blank? (we do it higher up in Login Controller)
   </details>
   <!-- vm.new_user = {} -->

   <details><summary>TODO #5 Hint</summary>
   
    - Inject $location into your controller
    
    - Pass the `'/profile'` path to the function in this [$location documentation](https://docs.angularjs.org/api/ng/service/$location#path)
    </details>
    <!-- $location.path('/profile') -->

3. Click the "Log Out" link to logout (TODO #6) and make sure it redirects to `/login` (TODO #7).

    <details><summary>TODO #6 Hint:</summary>
      
      - Look at the login method above for a pattern to follow (you only need one anonymous function here, no need for `onSuccess` and `onError`).

      - Check out [this documentation](https://github.com/sahat/satellizer#authlogout) for a method we can use instead of `login`
    </details>
    <!--return (
        $auth
          .logout() // delete token
          .then(function() {
            self.user = null;
          })
    )-->
    <details><summary>TODO #7 Hint:</summary>
    
      - Inject $location into this controller also
      
      - Pass `'/login'` to the `.path` method we used in #5
    </details>
    <!-- $location.path('/login') -->

  <!--10:35 15 minutes -->

4. Implement the functionality outlined in `Account.signup()` (TODO #8, #9, #10).

    <details><summary>TODO #8 Hint</summary>
    
     - Use your `login()` function as a pattern
     
     - Use the documentation referenced to build out the rest.
    </details>
    <!--     return (
      $auth
        .signup(userData)
        .then(
          function onSuccess(response) {
            $auth.setToken(response);
            //OR $auth.setToken(response.data.token);
          },
          function onError(error) {
            console.log(error);
          }
        )
    ); -->

    <details><summary>TODO #9 Hint</summary>
    
     - Remember what we did in #4?
    </details>
    <!-- inject $location into controller then vm.new_user = {} -->


    <details><summary>TODO #10 Hint</summary>
    
     - Remember what we did in #5?
    </details>
    <!-- $location.path('/profile') -->

5. At this point, you should be able to sign up a user, log them in, and view their profile page from the client.

<!-- If we have time to intro great, otherwise point these out as further exercises -->

<!--10:50 15 minutes -->

## User Settings

1. Add a `username` field to the Sign Up form, and add the `username` attribute to `User` model (server-side). Sign up a new user with a `username` (TODO #11 in signup.html, #12 in models/user.js).

    <details><summary>TODO #11 Hint</summary>
    
    ```html
    <div class="form-group">
        <input type="text" ng-model="sc.new_user.username" class="form-control" placeholder="Username">
    </div>
     ```

    </details>
 
2. On the user profile page (`profile.html`), make a form to edit the user's details. The form should initially be hidden, and when the user clicks a button or link to "Edit Profile", the form should show (**Hint:** `ng-show`) (TODO #13).

3. When the user submits the form, it should call a function in the `ProfileCtrl` (**Hint:** `ng-submit`). The function should send an `$http.put` request to `/api/me`. Verify that this works. (TODO #14)

## Nested Resources: Posts (TODO #15)
> No more TODOs! You're on your own from here on out!

<!--11:05 Intro these and then break -->

1. Create a form on the homepage for the user to add a blog-post (that's right - you're turning your Angular app into a microblog). The blog-post form should have input (`title`) and textarea (`content`) fields. Hint: Use `ng-model`.

3. Only show the form if there is a `currentUser` logged in.

4. Use the `ng-submit` directive to run a function called `createPost` when the user submits the form.

5. `createPost` should make an `$http.post` request to `/api/posts` (which isn't defined on the server yet!) with the `vm.post` object.

6. The next step is to implement posts on the server. First, create a Mongoose model `Post` (`models/post.js`).

7. A user should have many posts, so add an attribute to the `User` model called `posts` that references the `Post` model:

  ```js
  /*
   * models/user.js
   */

   var userSchema = new Schema({
     ...
     posts: [{ type: Schema.Types.ObjectId, ref: 'Post' }]
   });
  ```

8. In `server.js`, define two new API routes:
  * `GET /api/posts` should retrieve all the posts from the database.
  * `POST /api/posts` should create a new post that *belongs to* the current user (**Hint:** Use the `auth.ensureAuthenticated` function in the route to find the current user).

9. Refresh `localhost:9000` in the browser. Make sure you have a user logged in, and try to create a new post. Check the Chrome developer tools to make sure it's working.

## Finishing Touches

1. Validate blog-posts! Ensure a user can't submit an empty title or content. (Use both backend AND frontend validations).

2. On the user's profile page, display the number of posts the user has written. **Hint:** You'll need to add `.populate('posts')` to your `GET /api/me` route in `server.js`.

3. On the user profile page, the "Joined" date isn't formatted very nicely. Use Angular's built-in <a href="https://docs.angularjs.org/api/ng/filter/date" target="_blank">date filter</a> to display the date in this format: `January 25, 2016`.

## Resources

### Background Reading on JWTs:
##### What is JWT?
 * <a href="http://jwt.io" target="_blank">JSON Web Tokens</a>
 * <a href="https://scotch.io/tutorials/the-anatomy-of-a-json-web-token" target="_blank">The Anatomy of a JSON Web Token</a>

#### When would you use JWT and why? (Pros and cons)
 * <a href="https://developer.atlassian.com/static/connect/docs/latest/concepts/understanding-jwt.html" target="_blank">Understanding JWT</a>
 * <a href="https://auth0.com/blog/2015/10/07/refresh-tokens-what-are-they-and-when-to-use-them/" target="_blank">When to use JWT and why?</a>

#### Compare & contrast with cookies and/or sessions
 * <a href="https://auth0.com/blog/2014/01/07/angularjs-authentication-with-cookies-vs-token" target="_blank">Cookies vs Tokens. Getting auth right with Angular.JS</a>
 * <a href="https://scotch.io/tutorials/the-ins-and-outs-of-token-based-authentication" target="_blank">Ins-and-outs of token-based auth</a>

### Satellizer
 * <a href="https://github.com/sahat/satellizer#authloginuser-options" target="_blank">Satellizer Docs</a>

# HW 14 Code Walkthrough

## root directory
### server.js
Server.js is the main Javascript file, run to launch the server that hosts the site.

First, it brings in all the packages needed for the application to run. Those packages are:

Required directly through NPM (require("package-name")):
Express, for running the server
Express-Session, for using a concept called /sessions/ to track whether a user is logged in. A session consists of a session ID, which is sent by the browser to the server, and session variables, which are stored by the server. This allows an application to (at least a little) securely handle sensitive information, by keeping it on the server and only giving it to a user with a proper session ID. This also allows an individual user to stay logged in on a website after closing the page and returning.

Required from a file:
Passport, an NPM package that allows a session-based model of user authentication. The passport.js file is used to set up what constitutes valid authentication (an email that exists in the DB, the password that matches that email), and to link Passport to the DB using Sequelize.
Sequelize, an NPM package that makes dealing with SQL databases easier. The index.js file in the models folder is used to initialize some settings based on the config.json file in the config folder and connect to the correct DB.

It then sets the port variable and requires the Sequelize connection and package from the DB foler.

It then creates the express app and uses some middleware (urlencoded, json) for processing requests and the static middleware for setting the filepath from which to deliver files.

It then initializes passport and begins a session. It then requires the appropriate routes.

Finally, it synchronizes the Sequelize connection with the existing DB, and begins listening for requests to the specified port.

### package.json
Package.json describes this application. It lists information about the application, the author, the license, and the application's dependencies.

### package-lock.json
Package-lock.json records the exact version numbers of the packages used in the application, in case they are re-installed later. It is not necessary, but is recommended.

## config folder
The config folder contains the config.json for the application, the passport.js file that initializes Passport, and a middleware folder.

### config.json
The config.json file containst the settings that will be used to configure the Sequelize package, organized by "environments" (development, test, and production).

### passport.js
Passport.js initializes and configures Passport for our application. Passport, as described in the server.js description, is a session-based tool for user authentication. In the passport.js file, we tell Passport to use a LocalStrategy (taken from the passport-local package) to perform authentication. We define the strategy- we write a function that will return either "false" or a user object based on a DB call to check the username and password. We then define some boilerplate functions (serializeUser and deserializeUser) and export the configured Passport instance.

### middleware folder/isAuthenticated.js
The middleware folder in the config folder contains one file, isAuthenticated.js. This defines a function which is used by the application router to check whether a user is logged in. This prevents the user from bypassing authentication by accessing a route that should be gated behind the sign-in page directly. If the user attempts to access such a route, and is not authenticated, they will be redirected back to the sign-in page.

## models folder
The models folder contains our sequelize instance and the User Sequelize model.

### index.js
The index.js file in the models folders creates our Sequelize instance and connection, and configures them based on the config.json file in the config folder. It also uses the fs readdirSync function to import each model in the models folder to the Sequelize instance  and associate them before exporting the instance.

### user.js
The user.js file creates the User model for our Sequelize instance and our database. It includes an npm package, bcryptjs, so that we only store an encrypted "hash" of a password in our database, not the password itself. It also defines what fields the users table should contain, and the requirement that the email the user submits must be a valid email.

## node-modules folder
Contains the source code for all the npm packages (and their dependencies, and their dependencies' dependencies...) in the application.

## public folder
Contains the code the browser can see (hence, public).

### js folder / login.js
Contains some javascript code and functions to get the values from the form fields on the login page, and submit them to the appropriate routes to log the user in.

### js folder / members.js
Contains a function to get the current user and update the member name on the page.

### js folder / signup.js
Contains some javascript code and functions to get the values from the form fields on the signup page, and submit them to the appropriate routes to sign up a new user.

### stylesheets folder / style.css
Contains the custom CSS for the application- just one line adding margin-top of 50px to the login and signup forms.

### login.html
Contains the HTML for the login page.

### members.html
Contains the HTML for the welcome page.

### signup.html
Contains the HTML for the signup page.

## routes folder
Contains all the javascript defining API and HTML routes for the application (for browser to server communication).

### api-routes.js
Exports a function that adds all the API routes in the application to an express app passed to it.

### html-routes.js
Exports a function that adds all the HTML routes in the application to an express app passed to it.
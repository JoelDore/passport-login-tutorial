# Passport Login Tutorial

[![Read on Google Docs](https://img.shields.io/badge/Read%20on%20Google%20Docs%20-%E2%96%B6-blue)](https://docs.google.com/document/d/14gYYU2WqH39sHpWR-m_kcMSPKyf4BrjqeibTlxZ-imA/edit?usp=sharing)

## Under The Hood
This Node application uses an **Express** backend with **Passport** and **bcrypt** for secure user authentication, **Sequelize** to query and route data with a **MySQL** database, and **jQuery** to interact with the backend and dynamically generate **HTML** in **Bootstrap**-styled web pages.

---

## File Map

**config/**
* middleware/
    * [isAuthenticated.js](#Controllers)
* [config.json](#Database-Model)
* [passport.js](#Server)

**models/**
* [user.js](#Database-Model)
* [index.js](#Database-Model)

**public/**
* js/
    * [login.js](#Views)
    * [signup.js](#Views)
    * [members.js](#Views)
* stylesheets/
    * [style.css](#Views)
* [login.html](#Views)
* [signup.html](#Views)
* [members.html](#Views)

**routes/**
* [api-routes.js](#Controllers)
* [html-routes.js](#Controllers)
  
[server.js](#Server)

[package.json](#Server)

---
---

## Server
The dependencies for this app are installed based on metadata found in `package.json`.
The app is started by running `server.js` which creates a new **Express** server on the specified port, syncs with our database using **Sequelize** and initializes our **Passport** login system configuration (specified in `passport.js`).

## Database Model
Our database is set up using the specifications in `config.json` and synced with all defined models (representing database tables) in our `models/ `directory. 
The `index.js` file is a generic file that does the work of instantiating **Sequelize** and exporting all models in the directory in a single “db” object so we can easily interact with our database throughout the app. 
Our model `user.js` uses **bcrypt**, an npm package that hashes and compares passwords for added security.

## Views
The `public/` directory contains all the files that are seen or controlled directly by the user:

* `signup.html` is where the user creates a new account.
* `login.html` is where the user logs in to an existing account.
* `members.html` is the home page the user sees once logged in.

These pages are styled inline using the **Bootstrap** CDN, and `stylesheets/style.css` which adjusts a margin on the login/signup forms.
Our html files are able to communicate with our database by sending POST requests to the `apiRoutes.js` file via their respective javascript files in the `public/js/` directory:

* `login.js` and `signup.js` each capture and validate form inputs, make a POST request with the input values stored in a user object, and redirect the user to the appropriate page if successful.
* `members.js` simply makes a GET request for the logged-in user’s data to dynamically display their email in html.

## Controllers
The public web pages are loaded when `htmlRoutes.js` receives a GET request from the browser and delivers the corresponding html page. 
The route for our `members.html` page requires our `isAuthenticated.js` middleware which restricts the page to be accessible only by logged-in users.) 

Our `apiRoutes.js` file receives these GET/POST requests and handles each appropriately:

* Handles POST requests from `login.js` by using passport.authenticate middleware to validate the user’s login credentials before sending them back as a response.
* Handles POST requests from `signup.js` by automatically hashing the password based on our user.js model, creating a new user in our db, and logging in the user if successful.
* Handles GET requests from `members.js` by sending the user’s email and id as a response.
* The _/logout_ route (requested via button click in `members.html`) terminates the user’s login session.

---
---

## Contributing

**If you would like to add any changes to this project, follow the contribution guidelines below**

1. Fork this repository on GitHub and clone to your remote device

2. Install all dependencies:

        npm install

3. Create a new branch for your feature:

        git checkout -b feature/featureName

4. Commit/push your changes to your branch:

        git add .
        git commit -m 'Added this cool feature'
        git push --set-upstream origin feature/featureName

5. Create a new pull request with a comment describing your changes.

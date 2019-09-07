# **WORK IN PROGRESS**

Status Update
- [X] Hasura: Create add basic tables and function for anonymous access from Flutter app to test connectivity
- [X] Flutter: Create intial app
- [X] Hasura: Create ToDo tables for persistence (protected) and query results (public)
- [X] Hasura: Create aggregate function for general statistics (public)
- [ ] Flutter: Implement subscription to aggregate function
  - [X] Create class for aggregate view
  - [ ] Create subscription to aggregate view (Query is working in flutter, now adding subscriptions)
- [ ] Flutter: Create anonymous online view of aggregate view
  - [X] Sign-In link
  - [ ] Disparate colour scheme
  - [X] Display of ~subscribed~ [currently queried] general aggregate data
- [X] FusionAuth: Create initial users
- [ ] Flutter: Create Sign-In flow to FusionAuth
- [ ] Flutter: Create User Profile page
- [ ] Flutter: Create User online view of aggregate view
- [ ] Flutter: Create Admin online view of aggregate view
- [X] Flutter: Create placeholder pages for User features
- [X] Flutter: Create placeholder pages for Admin features
- [ ] Flutter: Create user and admin navigation tabs
- [X] Hasura: Create Announcments tables for persistence and query results
- [ ] Flutter: Implement subscription to announcment function
- [X] Hasura: Create admin functions for announcments
- [ ] Flutter: Improve Admin Announcments fuctionality
- [X] Hasura: Create ToDo tables for persistence and query results
- [ ] Flutter: Implement ToDo functionality into app

---

# ToDoTaDa
A vidtorial and codebase for a full-stack Flutter (UX) + Hasura (GraphQL) + FusionAuth (JWT) real-time "Competitive ToDo" app implementing a one-way data flow architecture (similar to Flux, React, Redux, etc, as opposed to MVC and unidirectional).

This repo, and accompanying Vidtorial, demonstrates the complete configuration, build, design, and deployment of a real-time Competitive ToDo app.

---
**Table of Contents**
1. [General Problem](#general-problem)
1. [Premise of Solution](#premise-of-solution)
1. [General Installation Steps](#installation-steps-summary)
1. [Summary of Development](#development-summary)
1. [Architecture & Design](#architecture--design)
---

## General Problem
ToDo app examples today struggle to demonstrate the real-time value that we've been evaluating for our latest nextgen projects.  So we end up trying a plaform or component down to the point where we discover it doesn't really work for anything much better than a blog.  We don't want another wordpress solution, and we don't want to build our own.  So we believe that we've come up with a good componentized open source stack that we can demonstrate the core objectives of our most complex projects to ourselves, but demonstrated as a simple ToDo app.  A simple as possible, but not any simpler, of course.

The limitations we've run into are always centred around one or more of the following:
* User management, and especially migration/integration from/with legacy systems
  * Some projects have lots of users to migrate over from older systems
  * Users shouldn't need to have the Authn/Autho UX negatively impacted just because a new implementation of the systems are deployed
* Thoroughly deep and complex subscriptions/live-queries
  * Projections, aggregations, and deep links need to be tracked in a subscription's implementation
  * Hasura has provided the best solution that we've come across to date
* Unified UX development efforts mobile, desktop, web browser, embedded
  * We don't want diverse teams working on diverse codebases with diverse functionalities
  * Flutter, with their announcement of commitment to Hummingbird, this is now a reality

## Premise of Solution
ToDoTaDa is an interactive platform that allows admin users to create todos for registered users to compete with each other to finish first and on-time.  The Admin user sets the datetime of when the todo needs to be completed by (todo expires), how many concurrent users can claim the todo to work on (concurrent competitors), and how long a user actually has to complete the work once claimed (claim expires).  A user will have one way to "succeed" at a todo (i.e. claim it and complete it before either time runs out or another user completes it), but several ways to "fail" at a todo (i.e. the todo expires, the claim expires, or another user completes the todo within the constraints).  Statistics will be available generally to any user of the app, including anonymous users.  But more advanced user and todo specific statistics will be available to users and admins, respectively.  Finally, a notification layer will overlay the UX that allows Admin users to send a pop-up message to any or all user groups, including Anon, User, and Admin.

This will demonstrate the following key objectives, which are all best experienced with "real-time" interactivity:
* A UX that demonstrates velocity and scale-of-use for anon users, registered users, and admin users
  * Why: simple authn/autho searpation of concerns within stack
* Demonstrate variable conditional-limitations, invoking understandable competition between users 
  * Why: real-time info is nice, but unless complexity in the variance and scope can be made understandable to the users, it won't be delightful to use, and thus not favourable to invoking a successful and necessary gamified experience
* Demonstrate competitive and time-boxed contexts, which require strategic planning and execution on several levels
  * Why: complexity of variance and scope are beyond a single context, and a UX must be able enable converged contexts for both task and organizational level constraints to enable wholistic, strategic decisions
* Demonstrate "elegant" private/public announcements in a real-time one-way environment
  * Why: targeted announcements within real-time systems seems overly susceptible to negatively impacting the UX, without building a complicated announcement system that works across platforms.  Is it possible to push announcements to the UI and let that app show it as soon as elegantly-possible?  What does that even mean?
* Componentize the entire stack between Frontend, GraphQL API, and Authentication
  * Why: while this stack will work well for net-new projects, it will need to support replacing any given component with an existing project-provided component.
* Understand the complexity, scalability, and reasonableness of disparate GraphQL subscriptions converged into a beautiful UX
  * Why: will the solution scale to millions of users, and dozens of converged subscriptions, and how.

To solve for the objectives, the goal is to build a full-stack codebase that demonstrates the following key outcomes:
* A beautiful Flutter app, initially targeted at mobile:
  * UX that starts at app launch
    * This is a device-level splash screen with fixed/disconnect display
  * App Splash-Screen that tests for connectivity and branches to offline or online mode
    * This is an app-level splash screen which is offline-first functional
  * Simple UX for Anonymous users
    * View general real-time statistics of users and todos
  * Interactive UX for Registered users
    * View available todos with the constraints
    * Claim todos to compete on
    * Complete claimed todos
    * Lose a claimed todo
    * Be informed of both predictive and pending risk
    * View personal and anonymous level statistics
  * Administrative UX for Admin users
    * Create todos and setting the competitive constraints
    * Create announcements and setting the groups to receive
    * View general real-time statistics of users and todos
    * View admin level statistics of each todo
  * An Offline UX at launch
    * Notify as offline at the App Splash-Screen
    * Displaying retry timer
    * Retry now button to override retry timer

# Installation Steps Summary
The order of the steps below follow the agile premise demonstrated in the Vidtorial and are not necessarily the most optimial sequence for a fully pre-designed application.

* Deploy FusionAuth (or any other asymmetric JWT provider)
  * Launch VM (link for auto-deploy at Azure in /docs, soon)
  * Configure domain name (details coming to /docs, soon)
  * Open ports:
    * 80 - for letsencrypt challenge only
    * 443 - for all public access
    * 22 - for internal access only
  * Configure Environment file (details coming to /docs, soon)
  * Run auto-install script in /docs (soon) which does:
    * docker, docker-compose install
    * Configures .yaml and runs docker-compose
    * Install nginx
    * Install and run letsencrypt
    * Configure nginx proxy to FusionAuth
  * Close port 80
  * Login to FusionAuth and configure (/docs, soon)
* Deploy Hasura+Caddy
  * Launch VM (Azure link, soon)
  * Configure domain name (soon)
  * Open ports:
    * 80 - for letsencrypt challenge only
    * 443 - for all public access
    * 22 - for internal access only
  * Configure Environment file (/docs, soon)
  * Run auto-install script in /docs (soon) which does:
    * docker, docker-compose install
    * Configures .yaml and runs docker-compose
  * Close port 80
  * Login to Hasura console with admin key

## Development Summary
* Hasura: Create add basic tables and function for anonymous access from Flutter app to test connectivity
  * Create a protected app table (/docs/vidtorial, coming soon)
  * Create a public query results table
  * Create a public app_version_check function
  * Insert one row
  * Test query to function by "anon"
* Flutter: Create intial app
  * Create a device-level splash screen
  * Create an app-level splash screen
  * Integrate GraphQL subscription to check for connectivity to server
  * Create an online view placeholder
  * Create an offline view with retry logic
* Hasura: Create ToDo tables for persistence (protected) and query results (public)
* Hasura: Create aggregate function for general statistics (public)
* Flutter: Implement subscription to aggregate function
  * Create class for aggregate view
  * Create subscription to aggregate view
* Flutter: Create anonymous online view of aggregate view
  * Sign-In link
  * Disparate colour scheme
  * Display of subscribed general aggregate data
* FusionAuth: Create initial users
  * user1 (role: app_admin) - don't give system admin privleges, it's just a different user type
  * user2 (role: user)
  * user3 (role: user)
* Flutter: Create Sign-In flow to FusionAuth
* Flutter: Create User Profile page
  * List JWT token data (roles, name, user-id)
  * Sign-out functionality
* Flutter: Create User online view of aggregate view
  * Access to profile view
  * Colour scheme variance from anon
  * Display of subscribed general aggregate data
* Flutter: Create Admin online view of aggregate view
  * Access to profile view
  * Colour scheme variance from anon and user
  * Display of subscribed general aggregate data
* Flutter: Create placeholder pages for User features
  * My ToDo's
  * Available ToDo's
* Flutter: Create placeholder pages for Admin features
  * ToDo's
  * Announcments
* Flutter: Create user and admin navigation tabs
  * User: My ToDo's, Available ToDo's, Main Stats(original aggregate view)
  * Admin: ToDo's, Announcments, Main Stats(original aggregate view)
* Hasura: Create Announcments tables for persistence and query results
  * Create function to return announcements based on the JWT-role and JWT-date without author_id or roles
  * Populate an announcement for the Anon role
* Flutter: Implement subscription to announcment function
  * Create class for announcments (w/ nullable author, roles)
  * Create subscription to role_based annoucnments function
  * Create logic to show announcments only once based on local state
  * Test anon, user, admin announcment combinations with manually populated announcments in Hasura
* Hasura: Create admin functions for announcments
  * Create function to list all announcemnts including author_id and roles
  * Create function to add a new announcement locked to 
* Flutter: Improve Admin Announcments fuctionality
  * Create new subscription to all announcments
  * Create mutation function to insert new announcement
  * Update the Admin's Announcments page to display all announcements history
  * Add modal page to capture new announcment in a form
  * Add an action button to the Admin's announcments page to access new announcment form
  * Test Announcement creation
* Hasura: Create ToDo tables for persistence and query results
  * Create function to return ToDo's to JWT-roles: app_admin
  * Create function to return ToDo's to a specific user_id flagged with "available" or "claimed"
  * Create function to return a specific ToDo
  * Create function to return aggregated statistics specific to a user_id
  * Create function to return a specific user's aggregated status
  * Populate a ToDo manually
* Flutter: Implement ToDo functionality into app
  * Create classes for ToDo list, ToDo item, User statistics, User status
  * Create subscription code to ToDo list (user, flag: available|claimed)
  * Create subscription code to ToDo item
  * Create subscription code to User statistics
  * Create subscription code to User status
  * Create mutation function to Insert a ToDo item
  * Create mutation function to Update (claim) a ToDo
  * Create mutation function to Update (complete) a ToDo
  * Update the User Available ToDo's page to subscribe to all ToDo's within "available" constraints for the user
  * Add a modal page to show an unclaimed ToDo to a user, with an action to triger the claim_todo function
  * Add a modal page to show a claimed ToDo to a user, with an action to trigger the claim_complete function
  * Update the User My ToDo's page to subscribe to all ToDo's withing "claimed" constraints for the user.
  * Add a modal page to capture new ToDo from in a form
  * Update Admin ToDo's page to display all ToDo's available to admin_role
  * Add an action button to the Admin ToDo's page to access new ToDo form
  * Test ToDo creation
  * Create widget in app bar for a user that subscribes to and indicates the User status (green, amber, red)

# Architecture & Design
## General Architecture
![Architecture](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/ToDoApp%20-%20Architecture.png)

![Blockitecture](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/ToDoApp%20-%20blockitecture.png)

## Design
Preload
![Preload](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/PRELOAD.png)

---

Splash
![Splash](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/SPLASH.png)

---

Offline View
![Offline View](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/OFFLINE%20VIEW.png)

---

Main Stats - Anonymous
![Main Stats - Anonymous](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/MAIN%20STATS%20-%20ANON.png)

---

Main Stats - User
![Main Stats - User](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/MAIN%20STATS%20-%20USER.png)

---

Main Stats - Admin
![Main Stats - Admin](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/MAIN%20STAT%20-%20ADMIN.png)

---

My ToDo's - User
![My ToDo's - User](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/MY%20TODO'S%20-%20USER.png)

---

Available ToDo's - User
![Available ToDo's - User](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/AVAILABLE%20TODO'S%20-%20USER.png)

---

ToDo's - Admin
![ToDo's - Admin](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/TODO'S%20-%20ADMIN.png)

---

Add ToDo - Admin
![Add ToDo - Admin](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/ADD%20TODO%20-%20ADMIN.png)

---

Claim ToDo - User
![Claim ToDo - User](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/CLAIM%20TODO%20-%20USER.png)

---

View ToDo - User
![View ToDo - User](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/VIEW%20TODO%20-%20USER.png)

---

Announcements - Admin
![Announcements - Admin](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/ANNOUNCEMENTS%20-%20ADMIN.png)

---

Add Announcment - Admin
![Add Announcment - Admin](https://github.com/lightcorelabs/todotada/raw/master/docs/assets/ADD%20ANCMT-%20ADMIN.png)

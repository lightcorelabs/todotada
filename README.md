# ToDoTaDa
A vidtorial and codebase for a full-stack Flutter (UX) + Hasura (GraphQL) + FusionAuth (JWT) real-time "Competitive ToDo" app implementing a one-way data flow architecture (similar to Flux, React, Redux, etc, as opposed to MVC and unidirectional).

This repo, and accompanying Vidtorial, demonstrates the complete configuration, build, design, and deployment of a real-time Competitive ToDo app.

## General Problem
ToDo app examples today suck at bringing out the real-time nature/value that we've been evaluating in our latest projects.  So we end up trying a plaform or component down to the point where we discover it doesn't really work for anything much better than a blog.  We don't want another wordpress solution, and we don't want to build our own.  So we believe that we've come up with a good componentized open source stack that we can demonstrate the core objectives of our most complex projects to ourselves, but demonstrated as a simple ToDo app.  A simple as possible, but not any simpler, of course.

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

## General Premise
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

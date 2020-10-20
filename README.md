# Full Stack Web Application -- Case Study for Per Scholas, September, 2020 -- Ghassan Nasr

## Introduction
* **Objective** - to create an implementation of a web service
* **Purpose** - to demonstrate the construction of a full-stacked web-application
* **Description**
	* This Case Study is a full-stack single-author blog application using a React.js front-end and a Spring MVC backend "microservice" that in turn connects to a MySQL or H2 database.
  * This repository DOES NOT represent the code repositories. Rather, it is an entrypoint for two code repositories for the frontend and backend application code repositories, respectively:
    * https://github.com/ghassannasr/perscholas.case-study-frontend
    * https://github.com/ghassannasr/perscholas.case-study-backend
  * The two applications were deployed to an AWS EC2 instance. 
  * The React Blog front-end application is running at: http://3.12.93.254:3000/blog/current
    * To log in, enter username "lorem" and password "ipsum"
  * The Spring MVC backend is running at http://3.12.93.254:8080 (additional path information needs to be appended to access the API)
  * The h2 console is not accessible on AWS (for security reasons).


## Usage and Deployment Overview (GIF Animations)

* **Blog App Sample Usage GIF**

    ![](./README_attachments/BlogAppUsage.gif)


* **Spring Boot Project Deployment GIF**

    ![](./README_attachments/SpringBootDeployRun6.gif)


* **React Blog App Deployment GIF**

    ![](./README_attachments/ReactDeployRun.gif)

## Usage Notes and Application Architecture and Data Model

* The data model provided by the Spring MVC backend is based on two entities (or tables in a relational database):
  * BlogPost entity with fields id, title, body, date (of post creation) and author. The "author" field is a foreign key to the blog post's author, of which there can only be one. 
  * Author entity with fields id, firstname, lastname, type, username, password. The "type" field identifies as either an "admin" or regular "user". The former has privileges for creating, editing, and deleting blog posts, while the latter can only read existing posts.
 * Author is thus in a one-to-many relationship with BlogPost.
 * Note: The data relationships/tables are not in proper normal form. In a noramlized database or entity relationship, there would be a third table/entity representing an Account. An Account would contain the username and password fields of Author, and a foreign key to the Account's owner (which would be an Author). An Author could in principle have more than one Account. For reasons of simplicity, the Account and Author information are merged into a single table/entity.
* The backend Spring Boot MVC exposes endpoints for two RESTful APIs, one for the BlogPost entity/model, and one for the Author entity/model. The endpoints (as paths appended to the URI) are:
  * /blogposts/get-all-posts: GET request that retrieves all blog posts
  * /blogposts/get-post/{id}: GET request that retrieves a blog post with a given id
  * /blogposts/create-post: POST request that creates a blog post per the request body JSON sent in the HTTP request header, of the form {id: , title: , body: , date: , author: }
  * /blogposts/delete-blogpost/{id}: DELETE request that deletes the blog post with the given id
  * /blogposts/update-blogpost/{id}: PUT request that updates the blogpost with the given id based on the blog fields attached as a JSON in the HTTP request body header
  * /authors/get-all-authors: GET request that retrieves all blog post authors
  * /authors/get-author/{id}: GET request that retrieves an author with a given id
  * /authors/create-author: POST request that creates an author per the request body sent in the HTTP request header, of the form {id: , firstname: , lastname: , type: , username: , password: }
  * /authors/delete-author/{id}: DELETE request that deletes the author with the given id
  * /authors/update-author/{id}: PUT request that updates the author with the given id based on the author fields attached as a JSON in the HTTP request body header




## Developmental Notes

### Tech Stack Selection
  * **Version Control System**
    * GitHub
    
  * **Frontend**
    * React (created with create-react-app for boilerplate)
    * Redux for central data store in application scope
    * React Bootstrap for styling
    * React Router for routing
    * Axios for RESTful API access
    * React HTML renderer (for rendering blog posts that contain HTML markup)
    * Both functional (Hooks) and class components
  
  * **Backend**
  * Spring Boot Back-end
    * Created with Spring Initialzr (https://start.spring.io/):
      * Java 1.8
      * Maven project
      * Spring Boot 2.3.4
      * Packaging: Jar
      * Added dependencies:
        * Spring Web
        * Spring Data Jpa
      * Added Mavem configuration/dependencies:
        * MySQL
        * Executable jar plugin
    
  * **Business Logic**
    * Java 1.8 (back-end)
      * IDE: IntelliJ Community Edition
    * JavaScript ES6 (front-end) 
      * IDE: Visual Studio Code with ESLint

  * **WebServer Implementation**
    * Spring Boot (internally uses Apache Webserver)

  * **Data Layer**
    * MySQL/MariaDB
    * H2 database and console
  
  * **Web Application Cloud Deployment**
    * AWS EC2 Instance:
      * Amazon Linux AMI, an EBS-backed, AWS-supported image. The default image includes AWS command line tools, Python, Ruby, Perl, and Java. The repositories include Docker, PHP, MySQL, PostgreSQL, and other packages.
      * Node.js (using nvm) was added.
      * Java was updated from the default 1.7 to 1.8

## Installation/Deployment

### Local Installation
* Frontend Installation using a GitHub clone of https://github.com/ghassannasr/perscholas.case-study-frontend
  * Clone the GitHub repository
  * Install the application using > npm install (or yarn install). This will install all the node modules as per the dependencies specified in package.json.
  * The appropriate IP address and port should be set in the file constants.js (for access to the backend API). For a local installation, the IP shoud be set to "localhost". The port should be set to 8080, which is the default used by the backend application. 

* Backend Installation using a GitHub clone of https://github.com/ghassannasr/perscholas.case-study-backend
  * Clone the GitHub repository
  * Run the Java the applicaton throught main entrypoint class as a Maven project. In most IDEs, there will be an option to open the project via the pom.xml files, which is the sound way to open the project.
  * The only configurable is the @CrossOrigin annotation in both the Author API controller and BlogPost API controller. The @CrossOrigin annotation contains the IP address and port on which the frontend is running. For local installation, the IP should be "localhost", and the port should be 3000, which is the default port for a React application.

### AWS Remote Installation/Deployment
* Remote deployment to AWS is detailed above in the deployment GIFs. Here are the steps:
  * Frontend Deployment:
    * The frontend React files are transferred (using Cyberduck ftp) to the AWS EC2 server instance (without the "node-modules" folder, which is enormous in size).
    * The node-modules are then generated by reading the dependencies and other directives from package.json by running the CLI command "npm install" (or "yarn install") in the main directory of the application code files.
    * The application server is then started with the command "npm start" (or "yarn start").
    * The only configurable aspect of the React app is the IP address and port that get included in the RESTful API calls/requests. The IP and port are managed centrally in a file named constants.js. (In the future will learn how to include in .env).
    * The application can then be started by navigating in the Web browser to the appropriate IP address and port, followed by the path to the main entry point, which is /blog/current.
  * Backend Deployment:
    * Two similar methods were used, one by deploying a traditional java JAR file, and another by deploying an executable JAR file.
    * In both cases, the JAR file is produced with Maven by running the CLI command "mvn package" in the main code source directory. In the case of this project (see the GIF above), the IntelliJ IDE was used to execute the mentioned command.
    * In order to make the JAR file executable, the "executable" configuration should be added to the pom.xml file inside the "spring-boot-maven-plugin" plugin (see the pom.xml). Without the "executable" configuration, the JAR file can be run as a traditional JAR with the "java" command. 
    * The Java application assumes Java 8 and up.


## Known Issues, Challenges, Discussion & Future Expectations

* Cross-table Queries:
  * Currently there is only one multi-table query joining blog posts to their author. However, there is only one author (also the administrator) for all blog posts. This is in line with the blog being a single author blog. The multi-author/admin functionality is partially implemented in that the central Redux store is initially populated with a array of administrators/authors (currently an array of one element). The application does iterate through the admin array to match blog post author id with author/admin id.
* Corner Cases:
  * Corner-case handling such as creating blank posts, deletion of post confirmation, cancelation of edits or new posts, etc. are not implemented, but the framework is there for easy addition.
* Application Glitches/Bugs:
  * The only known glitch thus far is when an administrator logs out when in the Login page view and then immediately tries to Submit another login attempt with the username and password fields already filled in with the correct credentials; the login in this case does not work, and thus the addition of the Refresh button (mainly for testing). The refresh invokes a callback function within the Login component that refreshes the admin information in the central Redux store (which get erased upon Logout). Such refresh functionality is not easily transmittable to the Logout functionality, because passing the callback function upward and across in the React component tree is not straight-forward. There is a way to register function references in the Redux store and invoke at will that way, but I found mixed reviews on implementing such a feature. This is my next challenge in learning more about React components talking to each other (other than the straight-forward parent transmitting callback references/handles down the child component tree as props.
* Data persistence:
  * At least three scopes of data persistence present themselves. 1) Backend database persistence, 2) Application scope data persistence, and 3) Component scope data persistence. My challenge was to determine how to manage data persistence among these three scopes. When an administrator logs in, their login information is kept in the central Redux store. One of the persisting questions (no pun intended!) was how to persist the actual blogposts. Should the blog posts be retrieved and and persisted to the backend database on each visit to the blog page (whether for reading, updating or creating new posts)? Or should the blog posts be retrieved only once during an application run and all blog posts (including updates, additions and deletions) be maintained in the central Redux store and then synchronized with the database at certain points during and before the application stops running? I decided on the former option because it ensures a consistent view of the database if an administor(s) in case of multiple concurrent sessions of the application. In its current iteration, the application only maintains central store data persistence to keep track of the current author/administrator. This was enough for me to practice implementation of component subscription to and dispatching events to the central store. And example of maintaining component scope data persistence is the Post component, which maintains state using Hooks.
* Database itegration:
  * One of the challenges for me was understanding how the Spring Boot Data Jpa model configuratons/annotations (especially those configuring the one-to-many/many-to-one relationship between the two database entities) translated in terms of the underlying ORM implementation and the lifecycle of the database during and after the execution of the application, and especially when interfacing with plain SQL (as opposed to JPQL or pure Java object manipulation); and example was one I used SQL to initialize the database by placing the SQL DDL in the automatically invoked (at application start-up) SQL script named data.sql. Ultimately, I was able to create the initial database using Java objects and straight Java handling of those objects and persisting to the implementation of the Jpa repositories. But not without some grief arising from errors having to do with object "detachment" and "persistence" or lack thereof. Admittedly, I had to massage the model annotations in ways based more on trial-and-error tweaks than a fundamental understanding of the workings of the underlying ORM. There are at least three or four different flavors of implementing relationships amongst tables/entities in the models and their supporting methods. 
  * Presently the application runs on the in-memory H2 database and MySQL, depending on which application.properties properties are activated. In development and testing, I am easily able to make use of the H2 Console and/or MySQLWorkbench to monitor the state of the database in either H2 or MySQL, respectively. In the AWS environment, however, I am not able to use the H2 Console, because of AWS security environment restrictions.
* React Component Rendering:
  * One of the fundamentals of React is understanding how and when components render and re-rerender. Because React prides itself on the sigle-page, render on-demand and only per component, design paradigm ... it becomes crucial to understand the events that trigger component rendering. A particular challenge in this regard was understanding the rendering process that is triggered when other asychronous threads are at work. This is nowhere more pertinent than in components that depend on asynchronous API calls that furnish the components with content to be rendered or that affects the conditions of the render. Instead of a class component, I leaned toward using a functional component and Hooks to manage and track state, and the fundamental useEffect() function that is crucial to synchronizing component renders with asynchronous threads. This aspect of managing state and rendering is nowhere more visible than in the Posts.js component. But what is fascinating to me is that there seem to be two distinct groupings of React developers: those who prefer to use functional components with Hooks to manage state, and those who prefer to use class components. There are many, including me, who use a combination of both.

## Sources Used and/or Consulted

There were many resources and tutorials I consulted for this project, not the least of which is my instructor Leon Hunter and his excellent introduction to Spring MVC architecture (in terms of conceptualizing repositories, services, models, and controllers). His introduction made it much simpler to both understand and build upon the boilerplate code generated by the Spring Initializr (mentioned above). In addition to the many sources I consulted, there are some that I used more directly, either as skeleton code or CSS styling that I adapted, or as textual content that I used verbatim as blog post filler content:

* The blog post content is taken directly from two sources (verbatim):
  * The Star Trek "Riker Ipsum" blog post content came from here:
    * https://www.mentalfloss.com/article/61003/10-funnier-alternatives-lorem-ipsum
  * The content of the other blog posts came from here: 
    * https://www.lipsum.com/
* "How to Use React Bootstrap with Redux" by Guarav Singhal (from Pluralsight)
  * This tutorial guide was my first introduction to Redux and React Bootstrap, and conveniently implemented a login feature as an example of using both (by maintaining the login information in the Redux store)
* I used this sample Bootstrap blog for blog presentation and styling: 
  * https://getbootstrap.com/docs/3.4/examples/blog/


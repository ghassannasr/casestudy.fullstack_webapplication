# Full Stack Web Application

## Introduction
* **Objective** - to create an implementation of a web service
* **Purpose** - to demonstrate the construction of a full-stacked web-application
* **Description**
	* This Case Study is a full-stack single-author blog application using a React.js front-end and a Spring MVC backend "microservice" that in turn connects to a MySQL database.
  * This repository DOES NOT represent the code repositories. Rather, it is an entrypoint for two code repositories, one for the front-end application, and the other for the back-end application.
    * https://github.com/ghassannasr/perscholas.case-study-frontend
    * https://github.com/ghassannasr/perscholas.case-study-backend
  * The two applications were deployed to an AWS EC2 instance. 
  * The React Blog front-end application is running at: http://3.12.93.254:3000/blog/current
    * To log in: username "lorem" and password "ipsum"
  * The Spring MVC back-end is running at http://3.12.93.254:8080 (additional path information needs to be appended to access the API)
  * The h2 console is not accessible on AWS (for security reasons).


## Overview (GIF Animations) of Usage & Deployment

* **Blog App Sample Usage GIF**

    ![](./README_attachments/BlogAppUsage.gif)


* **Spring Boot Project Deployment GIF**

    ![](./README_attachments/SpringBootDeployRun6.gif)


* **React Blog App Deployment GIF**

    ![](./README_attachments/ReactDeployRun.gif)


## Developmental Notes

### Tech Stack Selection
  * **Version Control System**
    * Github
    
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
      * Maven Project
      * Spring Boot 2.3.4
      * Packaging: Jar
      * Added dependencies:
        * Spring Web (??)
        * Spring Data Jpa (??)
      * Added Mavem configuration/dependencies:
        * MySQL
        * Executable jar plugin
    
  * **Business Logic**
    * Java 1.8 (back-end)
      * IDE: IntelliJ Community Edition
    * JavaScript ES6 (front-end) 
      * IDE: Visual Studio Code with ESLint

  * **WebServer Implementation**
    * Spring Boot (internally uses )

  * **Data Layer**
    * MySQL/MariaDB
    * H2 database and console
  
  * **Web Application Cloud Deployment**
    * AWS EC2 Instance:
      * Amazon Linux AMI, an EBS-backed, AWS-supported image. The default image includes AWS command line tools, Python, Ruby, Perl, and Java. The repositories include Docker, PHP, MySQL, PostgreSQL, and other packages.
      * In addition what comes with the AWS Linux AMI, Node.js (using nvm) is required.

## Installation/Deployment

### Local Installation
* Frontend Installation using a GitHub clone of https://github.com/ghassannasr/perscholas.case-study-frontend
* Backedn Installation using a GitHub clone of https://github.com/ghassannasr/perscholas.case-study-backend

### AWS Remote Installation/Deployment
* Remote deployment to AWS is detailed above in the deployment GIFs. Here are the steps:
  * Frontend Deployment:
   * The frontend React files are transferred (using Cyberduck ftp) to the AWS EC2 server instance (withode the "node-modules" folder, which is enormous in size).
   * The node-modules are then generated by reading the dependencies and other directives from package.json by running the CLI command "npm install" (or "yarn install") in the main directory of the application code files.
   * The application server is then started with the command "npm start" (or "yarn start").
   * The only configurable aspect of the React app is the IP address and port that get include in the RESTful API calls/requests. The IP and port are managed centrally in a file named constants.js. (In the future will learn how to include in .env).
   * The application can then be started by navigating in the Web browser to the appropriate IP address and port, followed by the path to the main entry point, which is /blog/current.
  * Backend Deployment:
   * Two similar methods were used, one by deploying a traditional java JAR file, and another by deploying an executable JAR file.
   * In both cases, the JAR file is produced with Maven by running the CLI command "mvn package" in the main code source directory. In the case of this project (see the GIF above), the IntelliJ IDE was used to execute the mentioned command.
   * In order to make the JAR file executable, the "executable" configuration should be added to the pom.xml file inside the "spring-boot-maven-plugin" plugin (see the pom.xml). Without the "executable" configuration, the JAR file can be run as a traditional JAR with the "java" command. 
   * The Java application assumes Java 8 and up.


## Known Issues, Challenges, Discussion & Future Expectations

* Cross-table Queries:
  * Currently the application does not require back-end queries with table joins. There is one author (also the administrator) for all blog posts. This is in line with the blog being a single author blog. However, the underlying application should ideally allow for easy extension to a multi-author blog; this would require an underlying database query that joing authors on blog posts, which does not presently exist. The multi-author/admin functionality is, however, partially implemented in that the central Redux store is initially populated with a array of administrators/authors (currently an array of one element). The application does iterate through the admin array to match blog post author id with author/admin id.
* Corner Cases:
  * Corner-case handling such as creating blank posts, deletion of post confirmation, cancelation of edits or new posts, etc. are not implemented, but the framework is there for easy addition.
* Application Glitches/Bugs:
  * The only known glitch thus far is when an administrator logs out when in the Login page view and then immediately tries to Submit another login attempt with the username and password fields already filled in with the correct credentials; the login in this case does not work, and thus the addition of the Refresh button (mainly for testing). The refresh invokes a callback function within the Login component that refreshes the admin information in the central Redux store (which get erased upon Logout). Such refresh functionality is not easily transmittable to the Logout functionality, because passing the callback function upward and across in the React component tree is not straight-forward. There is a way to register function references in the Redux store and invoke at will that way, but I found mixed reviews on implementing such a feature. This is my next challenge in learning more about React components talking to each other (other than the straight-forward parent transmitting callback references/handles down the child component tree as props.
* Data persistence:
  * At least three scopes of data persistence present themselves. 1) Backend database persistence, 2) Application scope data persistence, and 3) Component scope data persistence. My challenge was to determine how to manage data persistence among these three scopes. When an administrator logs in, their login information is kept in the central Redux store. One of the persisting questions (no pun intended!) was how to persist the actual blogposts. Should the blog posts be retrieved and and persisted to the backend database on each visit to the blog page (whether for reading, updating or creating new posts)? Or should the blog posts be retrieved only once during an application run and all blog posts (including updates, additions and deletions) be maintained in the central Redux store and then synchronized with the database at certain points during and before the application stops running? I decided on the former option because it ensures a consistent view of the database if an administor(s) in case of multiple concurrent sessions of the application. In its current iteration, the application only maintains central store data persistence to keep track of the current author/administrator. This was enough for me to practice implementation of component subscription to and dispatching events to the central store. And example of maintaining component scope data persistence is the Post component, which maintains state using Hooks.
* Database itegration:
  * One of the challenges for me was understanding how the Spring Boot Data Jpa model configuratons/annotations (especially those configuring the one-to-many/many-to-one relationship between the two database entities) translated in terms of the underlying ORM implementation and the lifecycle of the database during and after the execution of the application, and especially when interfacing with plain SQL (as opposed to JPQL or pure Java object manipulation); and example was one I used SQL to initialize the database by placing the SQL DDL in the automatically invoked (at application start-up) SQL script named data.sql. Ultimately, I was able to create the initial database using Java objects and straight Java handling of those objects and persisting to the implementation of the Jpa repositories. But not without some grief arising from errors having to do with object "detachment" and "persistence" or lack thereof. Admittedly, I had to massage the model annotations in ways based more on trial-and-error tweaks than a fundamental understanding of the workings of the underlying ORM. There are at least three or four different flavors of implementing relationships amongst tables/entities in the models and their supporting methods. 
  * As a result of the afore-mentioned challenges, I decided to stick with the H2 in-memory database for application development and testing. At present, the MariabDB/MySQL database is accessible, but the properties set in the application.properties Spring Boot configuration file are not sufficient to creating and later dropping the database to match the lifecycle of the application.
  * Presently the application runs on the in-memory H2 database. In development and testing, I am easily able to make use of the H2 Console to monitor the state of the database. In the AWS environment, however, I am not able to use the H2 Console, because of AWS security environment restrictions.
* React Component Rendering:
  * One of the fundamentals of React is understanding how and when components render and re-rerender. Because React prides itself on the sigle-page, render on-demand and only per component, design paradigm ... it becomes crucial to understand the events that trigger component rendering. A particular challenge in this regard was understanding the rendering process that is triggered when other asychronous threads are at work. This is nowhere more pertinent than in components that depend on asynchronous API calls that furnish the components with content to be rendered or that affects the conditions of the render. Faced with a decision on whether to use a functional or class component in implementing a component involving asynchronous threads, I discovered that the class component approach utilizes an important component lifecycle method that has been deprecated: componentDidMount(). Instead of a class component, I therefore leaned toward using a functional component and Hooks to manage and track state, and the fundamental useEffect() function that is crucial to synchronizing component renders with asynchronous threads. This aspect of managing state and rendering is nowhere more visible than in the Posts.js component. But what is fascinating to me is that there seem to be two distinct groupings of React developers: those who prefer to use functional components with Hooks to manage state, and those who prefer to use class components. There are many, including me, who use a combination of both.
* Cloud9:

## Sources Used/Consulted
* The blog post content is taken directly from two sources (verbatim):
  * 


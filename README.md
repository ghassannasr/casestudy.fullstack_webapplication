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
      * I installed Node.js (using nvm).

## Installation/Deployment

### Local Installation
* Frontend Installation using a GitHub clone of https://github.com/ghassannasr/perscholas.case-study-frontend
* Backedn Installation using a GitHub clone of https://github.com/ghassannasr/perscholas.case-study-backend

### AWS Remote Installation/Deployment

## Known Issues, Challenges, Discussion & Future Expectations

* Cross-table Queries:
  * Currently the application does not require back-end queries with table joins. There is one author (also the administrator) for all blog posts. This is in line with the blog being a single author blog. However, the underlying application should ideally allow for easy extension to a multi-author blog; this would require an underlying database query that joing authors on blog posts, which does not presently exist. The multi-author/admin functionality is, however, partially implemented in that the central Redux store is initially populated with a array of administrators/authors (currently an array of one element). The application does iterate through the admin array to match blog post author id with author/admin id.
* Corner Cases:
  * Corner-case handling such as creating blank posts, deletion of post confirmation, cancelation of edits or new posts, etc. are not implemented, but the framework is there for easy addition.
* Application Glitches/Bugs:
  * The only known glitch thus far is when an administrator logs out when in the Login page view and then immediately tries to Submit another login attempt with the username and password fields already filled in with the correct credentials; the login in this case does not work, and thus the addition of the Refresh button (mainly for testing). The refresh invokes a callback function within the Login component that refreshes the admin information in the central Redux store (which get erased upon Logout). Such refresh functionality is not easily transmittable to the Logout functionality, because passing the callback function upward and across in the React component tree is not straight-forward. There is a way to register function references in the Redux store and invoke at will that way, but I found mixed reviews on implementing such a feature. This is my next challenge in learning more about React components talking to each other (other than the straight-forward parent transmitting callback references/handles down the child component tree as props.
* Data persistence:
  * At least three scopes of data persistence present themselves. 1) Backend database persistence, 2) Application scope data persistence, and 3) Component scope data persistence. My challenge was to determine how to manage data persistence among these three scopes. When an administrator logs in, their login information is kept in the central Redux store. One of the persisting questions (no pun intended!) was how to persist the actual blogposts. Should the blog posts be retrieved and and persisted to the backend database on each visit to the blog page (whether for reading, updating or creating new posts)? Or should the blog posts be retrieved only once during an application run and all blog posts (including updates, additions and deletions) be maintained in the central Redux store and then synchronized with the database at certain points during and before the application stops running? I decided on the former option because it ensures a consistent view of the database if an administor(s) in case of multiple concurrent sessions of the application. In its current iteration, the application only maintains central store data persistence to keep track of the current author/administrator. This was enough for me to practice implementation of component subscription to and dispatching events to the central store. And example of maintaining component scope data persistence is the Post component, which maintains state using Hooks.
* React Component Rendering:
  * 
* Spring Boot Data Jpa:
  * 
* Cloud9:

## Sources Used/Consulted


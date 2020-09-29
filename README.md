# Full Stack Web Application

* **Objective** - to create an implementation of a web service
* **Purpose** - to demonstrate the construction of a full-stacked web-application
* **Description**
	* This Case Study is a full-stack single-author blog application using a React.js front-end and a Spring MVC backend "microservice" that in turn connects to a MySQL database.
  * This repository DOES NOT represent the code repositories. Rather, it is an entrypoint for two code repositories, one for the front-end application, and the other for the back-end application.
  ** https://github.com/ghassannasr/perscholas.case-study-frontend
  ** https://github.com/ghassannasr/perscholas.case-study-backend
  * The two applications were deployed to an AWS EC2 instance. 
  * The React Blog front-end application is running at: http://3.12.93.254:3000/blog/current
  * The Spring MVC back-end is running at http://3.12.93.254:8080 (additional path information needs to be appended to access the API)
  * The h2 console is not accessible on AWS (there is a property that needs to be enabled in application.properties that I could not figure out yet).


* Spring Boot Project Deployment GIF:

    ![](./README_attachments/SpringBootDeployRun6.gif)

* React Blog App Deployment GIF:

    ![](./README_attachments/ReactDeployRun.gif)

* Blog App Sample Usage GIF:

    ![](./README_attachments/BlogAppUsage.gif)


* **Additional Resources**
	* [The Original Case Study Document](./case-study.pdf)
	* [Case Study Outline](./case-study-outline.pdf)
	* [Case Study Deliverables](./README_deliverables.md)
	* [Identifying Plagiarism](./README_plagiarism.md)
	* [Suggested Project Topics](./README_suggested-project-topics.md)



## Minimum Features
* `RESTful` web service which consumes requests from a front-end web application and caches each request and the respective response to a database.
* The application must support a login functionality.




## Developmental Notes

### Tech Stack Selection
* Select at least 1 technology from each of the following categories:
  * **Version Control System**
    1. Github
    2. Bitbucket
    
  * **Wireframe**
    1. Mockflow
    2. Balsamiq
    3. Lucidcharts

  * **Frontend**
    1. Java Server Pages
    
  * **Business Logic**
    1. Java
    2. TypeScript

  * **WebServer Implementation**
    1. Spring Boot
    2. At least 1 [backing service](https://12factor.net/backing-services) API

  * **Data Layer**
    1. MySQL
    2. PostgreSQL
    3. MariaDB

  * **Web Server Cloud Deployment**
    1. Heroku
    2. AWS EC2 Instance
  
  * **Web Application Cloud Deployment**
    1. Netlify
    2. AWS EC2 Instance




### Installation
* It is advised that you make install each of the following technologies to ensure that are at least accessible
  * Install [NodeJs](https://nodejs.org/en/).
  * Install [Angular](http://angular.io/).
  * Install [AngularCli](https://cli.angular.io/).

# Udagram Image Filtering Microservice

### GitHub and Git: 

* GitHub: [https://github.com/nicholaspretorius/cloud-nd-project2](https://github.com/nicholaspretorius/cloud-nd-project2)
* Git: `git clone https://github.com/nicholaspretorius/cloud-nd-project2.git` to clone the project.
* `git checkout dev` checkout the development branch.
* `git checkout -b featureName` create and checkout a feature branch.

*Please Note: Development must be done in the `dev` branch. The `master` branch is protected and requires a Pull Request (PR) to be submitted for code review and approval.*

### Database

The app uses a PostgreSQL database. 

### Environment Variables

This project makes use of environment variables that are used locally via the `dotenv` NPM module. These are exlcuded from the repo. The files are named: 

* .env
* .env.test
* .env.production

The files should contain values as follows: 

```
DB_HOST=
DB_USER=
DB_PASS=
DB_NAME=
DB_PORT=
DB_PROTOCOL=postgres
JWT_SECRET=
S3_BUCKET=
AWS_REGION=
AWS_PROFILE=
```

Note that on the Elastic Beanstalk environment, AWS_PROFILE must be set to "DEPLOYED". 

### Development Notes: 

You can run the application as follows: 

* `npm i` to install dependencies.
* `npm run dev` for development purposes.
* `npm test` to run tests in watch mode with coverage.
* `npm run build` to build the project for deployment.
* `eb deploy --profile aws_educate` to deploy the project to existing Elastic Beanstalk instance.
* `cd www && npm start` for running built project (i.e. how EB will run the app).

### Elastic Beanstalk

Successfully deployed instance: 

![Elastic Beanstalk Running Instance](/deployment_screenshots/elastic-beanstalk-screenshot.png)

Note: The command within the "Node command" field on AWS Elastic Beanstalk is: `node server.js`

### AWS Urls

* Root: 200 [/](http://project2-dev2.us-east-1.elasticbeanstalk.com/)
* Incorrect: 400 [/images/filtered?image_url](http://project2-dev2.us-east-1.elasticbeanstalk.com/images/filtered?image_url)
* Unprocessable: 422 [/images/filtered?image_url=blah](http://project2-dev2.us-east-1.elasticbeanstalk.com/images/filtered?image_url=blah)
* Correct: 200 [/images/filtered?image_url=https://cdn.pixabay.com/photo/2020/01/22/10/18/landscape-4784949_1280.jpg](http://project2-dev2.us-east-1.elasticbeanstalk.com/images/filtered?image_url=https://cdn.pixabay.com/photo/2020/01/22/10/18/landscape-4784949_1280.jpg) 


### AWS Educate Profile

You need to have an AWS credentials file setup with the appropriate details from the AWS Educate profile. You can find these by clicking on the link for "Account Details" in the Vocareum interface and then selecting AWS CLI "Show" link. Copy and paste those details into the the credentials file as indicated below: 

```
[aws_educate]
aws_access_key_id=keygoeshere
aws_secret_access_key=tokengoeshere
aws_session_token=tokengoeshere
```

### Elastic Beanstalk (EB )Commands

In order to create the EB environment, you need to run the following commands:

* `eb init --profile aws_educate` Initialises the EB environment (will create a .elasticbeanstalk folder with a config.yml file within it)
* `eb list --all` Lists all eb environments if any
* `npm run build` Build the artifact for deployment.
* `eb create --profile aws_educate`
* `eb deploy --profile aws_educate`
* `eb terminate --all` to terminate Elastic Beanstalk environments
* `eb abort` to cancel an active eb operation

Please note: In the .elasticbeanstalk/config.yml, you need to add the following property on the root level, after "branch-defaults": 

```
deploy:
  artifact: ./www/Archive.zip
```

## Original README Instructions

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into three parts:
1. [The Simple Frontend](https://github.com/udacity/cloud-developer/tree/master/course-02/exercises/udacity-c2-frontend)
A basic Ionic client web application which consumes the RestAPI Backend. [Covered in the course]
2. [The RestAPI Backend](https://github.com/udacity/cloud-developer/tree/master/course-02/exercises/udacity-c2-restapi), a Node-Express server which can be deployed to a cloud service. [Covered in the course]
3. [The Image Filtering Microservice](https://github.com/udacity/cloud-developer/tree/master/course-02/project/image-filter-starter-code), the final project for the course. It is a Node-Express application which runs a simple script to process images. [Your assignment]

## Tasks

### Setup Node Environment

You'll need to create a new node server. Open a new terminal within the project directory and run:

1. Initialize a new project: `npm i`
2. run the development server with `npm run dev`

### Create a new endpoint in the server.ts file

The starter code has a task for you to complete an endpoint in `./src/server.ts` which uses query parameter to download an image from a public URL, filter the image, and return the result.

We've included a few helper functions to handle some of these concepts and we're importing it for you at the top of the `./src/server.ts`  file.

```typescript
import {filterImageFromURL, deleteLocalFiles} from './util/util';
```

### Deploying your system

Follow the process described in the course to `eb init` a new application and `eb create` a new environment to deploy your image-filter service! Don't forget you can use `eb deploy` to push changes.

## Stand Out (Optional)

### Refactor the course RESTapi

If you're feeling up to it, refactor the course RESTapi to make a request to your newly provisioned image server.

### Authentication

Prevent requests without valid authentication headers.
> !!NOTE if you choose to submit this, make sure to add the token to the postman collection and export the postman collection file to your submission so we can review!

### Custom Domain Name

Add your own domain name and have it point to the running services (try adding a subdomain name to point to the processing server)
> !NOTE: Domain names are not included in AWS’ free tier and will incur a cost.

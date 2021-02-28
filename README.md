# Udagram Image Filtering Application

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into two parts:
1. Frontend - Angular web application built with Ionic Framework
2. Backend RESTful API - Node-Express application

## Getting Started
> _tip_: it's recommended that you start with getting the backend API running since the frontend web application depends on the API.

### Prerequisite
1. The depends on the Node Package Manager (NPM). You will need to download and install Node from [https://nodejs.com/en/download](https://nodejs.org/en/download/). This will allow you to be able to run `npm` commands.
2. Environment variables will need to be set. These environment variables include database connection details that should not be hard-coded into the application code.
#### Environment Script
A file named `set_env.sh` has been prepared as an optional tool to help you configure these variables on your local development environment.
 
We do _not_ want your credentials to be stored in git. After pulling this `starter` project, run the following command to tell git to stop tracking the script in git but keep it stored locally. This way, you can use the script for your convenience and reduce risk of exposing your credentials.
`git rm --cached set_env.sh`

Afterwards, we can prevent the file from being included in your solution by adding the file to our `.gitignore` file.

### Database
Create a PostgreSQL database either locally or on AWS RDS. Set the config values for environment variables prefixed with `POSTGRES_` in `set_env.sh`.

### S3
Create an AWS S3 bucket. Set the config values for environment variables prefixed with `AWS_` in `set_env.sh`.

### Backend API
* To download all the package dependencies, run the command from the directory `udagram-api/`:
    ```bash
    npm install .
    ```
* To run the application locally, run:
    ```bash
    npm run dev
    ```
* You can visit `http://localhost:8080/api/v0/feed` in your web browser to verify that the application is running. You should see a JSON payload. Feel free to play around with Postman to test the API's.

### Frontend App
* To download all the package dependencies, run the command from the directory `udagram-frontend/`:
    ```bash
    npm install .
    ```
* Install Ionic Framework's Command Line tools for us to build and run the application:
    ```bash
    npm install -g ionic
    ```
* Prepare your application by compiling them into static files.
    ```bash
    ionic build
    ```
* Run the application locally using files created from the `ionic build` command.
    ```bash
    ionic serve
    ```
* You can visit `http://localhost:8100` in your web browser to verify that the application is running. You should see a web interface.

## Tips
1. Take a look at `udagram-api` -- does it look like we can divide it into two modules to be deployed as separate microservices?
2. The `.dockerignore` file is included for your convenience to not copy `node_modules`. Copying this over into a Docker container might cause issues if your local environment is a different operating system than the Docker image (ex. Windows or MacOS vs. Linux).
3. It's useful to "lint" your code so that changes in the codebase adhere to a coding standard. This helps alleviate issues when developers use different styles of coding. `eslint` has been set up for TypeScript in the codebase for you. To lint your code, run the following:
    ```bash
    npx eslint --ext .js,.ts src/
    ```
    To have your code fixed automatically, run
    ```bash
    npx eslint --ext .js,.ts src/ --fix
    ```
4. Over time, our code will become outdated and inevitably run into security vulnerabilities. To address them, you can run:
    ```bash
    npm audit fix
    ```
5. In `set_env.sh`, environment variables are set with `export $VAR=value`. Setting it this way is not permanent; every time you open a new terminal, you will have to run `set_env.sh` to reconfigure your environment variables. To verify if your environment variable is set, you can check the variable with a command like `echo $POSTGRES_USERNAME`.

## Project rubric
### Containers and Microservices
* `/feed` and `/user` backends are separated into independent projects.

   - [`/feed`](https://github.com/giovanimachado/udagram-microservices/tree/main/udagram-api-feed) microservice! <br>
   - [`/user`](https://github.com/giovanimachado/udagram-microservices/tree/main/udagram-api-user) microservice! 

* Project includes Dockerfiles to successfully create Docker images for `/feed`, `/user` backends, project frontend, and reverse proxy. 

    Dockerfiles:<br> 
      - [`/feed`](https://github.com/giovanimachado/udagram-microservices/blob/main/udagram-api-feed/Dockerfile)<br> 
      - [`/user`](https://github.com/giovanimachado/udagram-microservices/blob/main/udagram-api-user/Dockerfile)<br> 
      - [frontend](https://github.com/giovanimachado/udagram-microservices/blob/main/udagram-frontend/Dockerfile)<br>
      - [reverse proxy](https://github.com/giovanimachado/udagram-microservices/blob/main/udagram-deployment/Docker/Dockerfile)<br>
     
    Setup Docker Environment:
    
     cd to [Dockerfolder](https://github.com/giovanimachado/udagram-microservices/tree/main/udagram-deployment/Docker)
    
     run:  
      `docker-compose -f docker-compose-build.yaml build --parallel` <br>
      `docker-compose -f docker-compose-build.yaml push`
    
    Screenshot:<br>
     
     ![Dockerhub](https://github.com/giovanimachado/udagram-microservices/blob/main/screenshots/1.2-Docker-hub.PNG)
    

### Independent Releases and Deployments
* Include a [`.travis.yml`](https://github.com/giovanimachado/udagram-microservices/blob/main/.travis.yml) file for CI.

    Screenshot:<br>
     
     ![Travis CI interface](https://github.com/giovanimachado/udagram-microservices/blob/main/screenshots/2.1-Travis-CI-interface_a.PNG)

### Service Orchestration with Kubernetes
* A screenshots of `kubectl` commands show the Frontend and API projects deployed in Kubernetes.

* The output of `kubectl get pods` indicates that the pods are running successfully with the STATUS value Running.

* The output of `kubectl describe services` does not expose any sensitive strings such as database passwords.
  
  It is necessary to create a kubernetes cluster to fill these requirements. To create a cluster run:
 
  `eksctl create cluster --name udagram-microservices --version 1.18 --nodegroup-name standard-workers --node-type t3.micro --nodes 6 --nodes-min 1 --nodes-max 6 --node-ami auto --region us-east-1`
  
  cd to [Kubernetes](https://github.com/giovanimachado/udagram-microservices/tree/main/udagram-deployment/Kubernetes) and run:
  
   `kubectl apply -f ./config` <br>
   `kubectl apply -f ./service` <br>
   `kubectl apply -f ./deployment`
   
    Screenshots:<br>
     
     Pods: <br>
     ![Pods](https://github.com/giovanimachado/udagram-microservices/blob/main/screenshots/3.1.1-kubectl-get-pods.PNG)
     
     No sensitive information: <br>
     ![No sensitive information](https://github.com/giovanimachado/udagram-microservices/blob/main/screenshots/3.1.2-kubectl-describe-services_a.PNG)
     
* Screenshot of Kubernetes services shows a reverse proxy

     ![everse proxy](https://github.com/giovanimachado/udagram-microservices/blob/main/screenshots/3.2-Screenshot-shows-reverse-proxy.PNG)

* Kubernetes services are replicated. At least one of the Kubernetes services has replicas: defined with a value greater than 1 in its [`deployment.yml`](https://github.com/giovanimachado/udagram-microservices/blob/main/udagram-deployment/Kubernetes/deployment/frontend-deployment.yaml) file.

* Screenshot of Kubernetes cluster of command `kubectl describe hpa` has autoscaling configured with CPU metrics.
 
  To create autoscale, run: `kubectl autoscale deployment backend-feed --cpu-percent=80 --min=2 --max=3` I decided to auscale the `feed` service.
  
    Screenshot:<br>
     
     ![Autoscale](https://github.com/giovanimachado/udagram-microservices/blob/main/screenshots/3.3.2-kubectl-describe-hpa.PNG)  

#### Debugging, Monitoring, and Logging

* Screenshot of one of the backend API pod logs indicates user activity that is logged when an API call is made.

  It is necessary to send requests to backend to generate the logs, then forward local ports for the reverseproxy and frontend services:
  
   `kubectl port-forward service/reverseproxy 8080:8080`<br>
   `kubectl port-forward service/frontend 8100:8100`
  
     ![App](https://github.com/giovanimachado/udagram-microservices/blob/main/screenshots/5-udagram.PNG)    
  
  To take the screenshot of the logs, run: `kubectl logs <pode name>`

    Screenshot:<br>
     
     ![logs](https://github.com/giovanimachado/udagram-microservices/blob/main/screenshots/4.0-%20API-pod-logs_a.PNG)  
  
I think you should use an
`<addr>` element here instead.

   ```bash
   kubectl port-forward service/reverseproxy 8080:8080
   ```


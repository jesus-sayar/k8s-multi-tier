# Deploying a Multi-Tier Application

The application consists of a backend database and a frontend. For the backend, we will be using a MongoDB database, and for the frontend, we have a Python Flask-based application.

## Deploy the Mongo database

First we create a deployment for Mongo database. After we create a service. We use the default ClusterIP ServiceType. This means that the mongodb Service will not be accessible from the external world.

`kubectl create namespace testing-jsayar`

`kubectl create -f rsvp-db-deployment.yaml`

`kubectl create -f rsvp-db-service.yaml`

## Deploy the Python Flask-Based Frontend

The frontend is created using a Python Flask-based microframework. Its source code is available [here](https://github.com/cloudyuga/rsvpapp). There are a Docker image called [teamcloudyuga/rsvpapp](https://hub.docker.com/r/teamcloudyuga/rsvpapp/), in which the application's source code is imported. The application's code is executed when a container created from that image runs. The Dockerfile to create the teamcloudyuga/rsvpapp image is available [here](https://raw.githubusercontent.com/cloudyuga/rsvpapp/master/Dockerfile).

While creating the Deployment for the frontend, we are passing the name of the MongoDB Service, mongodb, as an environment variable, which is expected by our frontend.

Notice that in the ports section we mentioned the containerPort 5000, and given it the web-port name. We will be using the referenced web-port name while creating the Service for the rsvp application. This is useful, as we can change the underlying containerPort without making any changes to our Service.

`kubectl create -f rsvp-web-deployment.yaml`

`kubectl create -f rsvp-web-service.yaml`


## Check Out the Available Deployments and Services

`kubectl get deployments`

`kubectl get services`

## Scale the Frontend

`kubectl scale --replicas=4 -f /tmp/rsvp-web.yaml`
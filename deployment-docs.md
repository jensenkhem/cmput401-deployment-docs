# Deployment Instructions
The following set of instructions will walk through the entire deployment process from start to finish, including the installation of any prerequisite software and deployment tools.

## Prerequisites

This application is built for and intended to be run on a Linux System.

Before proceeding further, it is important that the following tool(s) are installed and available for use on the target deployment host:

### Install Docker/NGINX

#### Docker
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)

The above links lead to the official documentation for installing Docker and Docker Compose. A simple test to verify if the above tools are installed, is to open a terminal and run the following commands:
```
$ docker -v
Docker version 20.10.17, build 100c701

$ docker compose version
Docker Compose version v2.10.2
```
If running the above commands does not result in output similar to above (the versions may be newer), then the necessary tools may not have been installed correctly.

#### NGINX
- [NGINX](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)

The above link leads to the installation guide for NGINX. A simple test to verify if NGINX was installed correctly, is to open a terminal and run the following commands:
```
$ nginx -v
nginx version: nginx/1.17.0
```
If running the above command does not result in output similar to above (the version may be newer), then NGINX may not have been installed correctly.

### Register a Google Cloud OAuth 2.0 Application
TODO

### Register a Github Oath 2.0 Application
TODO

### Configure Stripe Application and Product
TODO

### Clone the Repository
TODO

## Deployment Configuration
Once the `docker` and `docker compose` commands are available for use on your deployment host, we can begin to configure the application for deployment. 

Since this is a full-stack web application, there are four notable components of the application which have their own configuration values.
- NodeJS (Backend)
- ReactJS (Frontend)
- PostgreSQL (Database)
- NGINX (Web hosting)

Luckily, this application is entirely configurable via a set of expected environment variables, and therefore the configuration for these components is consolidated. 

Additionally, there are a few required Docker configuration options that will be explained below.
 
The following section will go over each component's configuration separately, although you may notice that the configuration files are shared between components. This is no issue, as each of the below sections will only cover the set of environment variables required by that specific component.

## Backend Configuration
All of the configuration variables associated with the backend should be configured under the cloned repository's `deployment/.env` file. Some of the configuration values below will have default values.
```
# Full URL (with port) that the application's backend will be
# accessible at. If deploying for local access,
# this value can left as the default. Otherwise, it should be
# set to the full URL where you expect your backend to be
# accessible externally.
BACKEND_URL=http://localhost:3000

# Full URL (with port) that the application's frontend will be
# accessible at from the backend. If deploying for local access,
# this value can left as the default. Otherwise, it should be
# set to the full URL where you expect your backend to be
# accessible externally.
FRONTEND_URL=http://localhost:8080

# Admin interface configuration
# This application comes with a built in 'admin' interface for the
# application. This interface will be available externally at 
# ${BACKEND_URL}/api/admin, and therefore
# requires a secure set of credentials for access. Ensure that these
# credentials are not commited to the repository or shared 
# with others.
ADMIN_EMAIL=
ADMIN_PASSWORD=


```

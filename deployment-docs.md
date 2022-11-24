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
Follow the 'Prerequisites' section of the [following guide](https://developers.google.com/identity/protocols/oauth2/javascript-implicit-flow), ensuring that the URLs that you choose here match that of your desired deployment URL, and that you save the secret key, and client ID in a secure location for later use.

### Register a Github Oath 2.0 Application
Follow [this guide](https://docs.github.com/en/developers/apps/building-oauth-apps/creating-an-oauth-app), again ensuring that the URLs chosen here match that of your desired deployment URL, and that you save the secret key, and client ID in a secure location for later use.

### Configure Stripe Application and Product
- [Create a Stripe account](https://dashboard.stripe.com/register) 
- [Create a monthly subscription 'product'](https://stripe.com/docs/billing/subscriptions/build-subscriptions?ui=checkout#create-pricing-model), making sure to save the corresponsing 'Price ID'.

### Clone the Repository
Clone out this repository on the deployment host in a location which you have permission.

## Deployment Configuration
With the prerequisites out of the way, we can begin to configure the application for deployment. 

Since this is a full-stack web application, there are four notable components of the application which have their own configuration values.
- NodeJS (Backend)
- ReactJS (Frontend)
- PostgreSQL (Database)
- NGINX (Web hosting)

Luckily, other than NGINX, this application is entirely configurable via a set of predefined environment variables, and therefore the configuration for these components is consolidated. 

Additionally, there are a few required Docker configuration options that will be explained at the end.
 
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

### Authentication Variables ###
## Google
# Variables associated with Google OAuth 2.0. 
# These values can be found on your google API dashboard 
# if you have registered the application correctly. 
# Again, ensure that the CLINET_ID and CLIENT_SECRET values are not # shared with anyone outside of this file.
GOOGLE_CLIENT_SECRET=
GOOGLE_CLIENT_ID=
GOOGLE_CALLBACK_URL=

## Github
# Variables associated with Github OAuth 2.0. 
# These values can be found under your Github Developer Settings 
# if you have registered the application correctly. 
# Again, ensure that the CLINET_ID and CLIENT_SECRET values are not # shared with anyone outside of this file.
GITHUB_CLIENT_SECRET=
GITHUB_CLIENT_ID=
GITHUB_CALLBACK_URL=

## Session Variables
# This variable is required to compute the hash for all 
# browser sessions created by the application.
# Ensure that this value is sufficiently long and random, and is not
# shared with anyone.
SESSION_SECRET=

### Stripe Variables ###
# Stripe Secret key 
# This the is the secret key associated with your Stripe account
# and application. It can be found on your Stripe dashboard
# and should not be shared with anyone.
STRIPE_SECRET_KEY=

# This the is the publishable key associated with your Stripe 
# account and application. It can be found on your Stripe dashboard
# and will be used on the front-end.
STRIPE_PUBLISHABLE_KEY=

# The Price ID of the subscription we created in the prerequisites
# section.
STRIPE_SUBSCRIPTION_ID=
```

## Frontend Configuration

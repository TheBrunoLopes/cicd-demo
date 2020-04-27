# Drone

It's mandatory to have the [.drone.yml](../../.drone.yml) file at the root of the project. This is that defines all the steps of the pipeline. The pipeline defined will build and push an image to a dockerhub repository. For simplicity, no secrets were created. You must edit the `.drone.yml` file with your Docker Hub credentials

## Executing pipeline locally

You will need to install the [Drone cli tool](https://docs.drone.io/cli/install/). 
Then you can execute the pipeline with

```bash
drone exec
```

This will execute the pipeline against the local version of the source code.

## Drone Server - Setup

This setup will enable you to have a Drone server, and build pipelines against your github repository

### Url

Use [ngrok](https://ngrok.com/download) to expose localhost ports to the web

```bash
ngrok http 80
```

from this part of the output: 

```text
(..)
Forwarding                    http://b9b6d034.ngrok.io -> http://localhost:80
(..)
```

this will be your url:
```text
b9b6d034.ngrok.io
```

### Github

You will need to setup an OAuth2 apps in Github. Go to `Settings > Developer Settings > OAuth Apps` and create a new app. 

You will have to put this the urls like this:
- Homepage URL: https://37e28648.ngrok.io/
- Authorization callback URL: https://37e28648.ngrok.io/login

After creating, copy the Client ID and Client Secret because they will be necessary to start the drone server 

### Run server

The Drone server can be ran locally with Docker images. You can easily bootstrap it with this [docker-compose.yml](docker-compose.yml). Include your url without the `https://` part 

```bash
URL=<your-url> \
  CLIENT_ID=<oauth2-client-id> \
  CLIENT_SECRET=<oauth2-client-secret> \
  docker-compose up
```


## Dashboard

You can also access your Drone server at your URL. You will need to login with your github account and you will see all the repositories that you have access to. You need to click on one and activate it. Activating a repository will automatically create a webhook. You can check it at the repository settings. 
If the webhook works, you can trigger a pipeline by pushing a commit, for example.

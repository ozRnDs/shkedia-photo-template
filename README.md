- [shkedia-photo-template](#shkedia-photo-template)
- [Overview](#overview)
- [Deploy](#deploy)
  - [Local Deployment](#local-deployment)
- [Development](#development)
  - [Environment](#environment)
  - [Build](#build)
  - [Test](#test)
    - [Running Tests](#running-tests)


# shkedia-photo-template
# Overview
This is a template for the project.  
It includes raw documentation structure and will grow in the future.  
Make sure to change the components name according to your need in all the scripts added to the template.  
The current template was copied from the start of the gallery backend project and the packages and naming suits that.

**Action to use template**
1. Update your requirements.txt
2. Update the variables names in the following files:
   1. README
   2. docker-compose
3. Update the entrypoint in the Dockerfile to fit your framework (DJange, Uvicorn (for FastAPI) or vanilla python)
4. Set the development and production ports in the following files:
   1. docker-compose
   2. .devcontainer/devcontainer.json


# Deploy
## Local Deployment
1. Set the location of the credentials files on the host:
    ```bash
    export GALLERY_BACKEND_VERSION=$(cz version --p)
    ```
1. Create credentials token files as follows:
    ```bash
    # Create the folder to be mounted to the container
    if [ ! -d $HOST_MOUNT ]; then
        sudo mkdir $HOST_MOUNT
        sudo chown $USER $HOST_MOUNT
    fi
1. Create *gallery_env.env* file in .local folder with the service variables:
    ```bash
    export CREDENTIALS_FOLDER_NAME=/temp
    export AUTH_DB_CREDENTIALS_LOCATION=$CREDENTIALS_FOLDER_NAME/postgres_credentials.json

    if [ ! -d .local ]; then
        sudo mkdir .local
    fi
    cat << EOT > .local/gallery_backend.env
    CREDENTIALS_FOLDER_NAME=$CREDENTIALS_FOLDER_NAME
    JWT_KEY_LOCATION=$CREDENTIALS_FOLDER_NAME/jwt_token
    EOT
    ```
1. Run the service using compose command:
    ```bash
    docker compose up -d
    ```
1. The env can be override by the following command:
    ```bash
    export GALLERY_ENV=.local/gallery_backend.env
    docker compose --env-file ${GALLERY_ENV} up -d
    ```

# Development
## Environment

## Build
1. Set the parameters for the build
    ```bash
    export GALLERY_BACKEND_VERSION=$(cz version -p)
    export IMAGE_NAME=shkedia-photo-gallery-backend:${GALLERY_BACKEND_VERSION}
    export IMAGE_FULL_NAME=public.ecr.aws/q2n5r5e8/ozrnds/${IMAGE_NAME}
    ```
2. Build the image
    ```bash
    docker build . -t ${IMAGE_FULL_NAME}
    ```
3. Push the image
    ```bash
    docker push ${IMAGE_FULL_NAME}
    ```
    Before pushing the image, make sure you are logged in

## Test
### Running Tests
1. Make sure you have all the requirements_dev.txt installed. It is essential for the tests
    ```bash
    pip install -r requirements_dev.txt
    ```
1. Run the tests using CLI
    ```bash
    pytest -s tests
    ```
    **IMPORTANT**: Many of the tests need a connection to the sql server as they are integration tests.
**NOTE**: It is possible and easy to run the tests using VScode. Just press the "play" arrow. All the configuration for it are in the .vscode folder. Just make sure to install the Python Extension

# Docker app image installation: How-to instruction

To begin the work, please install [Docker](https://www.docker.com/products/docker-desktop).
## Presetting
The point is the running of Docker application container in the Kubernetes cluster.
For example let's consider our application ExampleApp that accepts requests on port 8800.

It is required to create the following directory with subdirectories:
```sh
mkdir quickstart_docker/ # the entire project directory
mkdir quickstart_docker/application/ # the applicaion code directory
mkdir quickstart_docker/docker/ # the directory for the files related to Docker 
mkdir quickstart_docker/docker/application/ # the Dockerfile directory for the application
```
**Note**: all the directories can be included in the root, but it is recommended to follow the structure.
## Application deployment
It is required to have the application for its further deployment - let's take `application.py` file into  `quickstart_docker/application` directory:
```sh
import http.server
import socketserver

PORT = 8000

Handler = http.server.SimpleHTTPRequestHandler

httpd = socketserver.TCPServer(("", PORT), Handler)

print("serving at port", PORT)
httpd.serve_forever()
```
## Application environment
The application environment should include at least Python and the OS with dependencies. On  Docker Hub it is possible to find ready-made application images with Python.

In `quickstart_docker/docker/application` directory it is required to take the `Dockerfile` file with the following:
```
# Use base image from the registry
FROM python:3.5

# Set the working directory to /app
WORKDIR /app

# Copy the 'application' directory contents into the container at /app
COPY ./application /app

# Make port 8000 available to the world outside this container
EXPOSE 8000

# Execute 'python /app/application.py' when container launches
CMD ["python", "/app/application.py"]
```
The next step is to run the following in the terminal:
```
docker build . -f-docker/application/Dockerfile -t exampleapp
```
The arguments include:
- **.** is the build context
- **-f docker/application/Dockerfile** is the Docker file
- **-t exampleapp** is the image tag

See more information about the building for Docker application images [here](https://docs.docker.com/engine/reference/builder/).

Then it is possible to see the list of application images:
```
$ docker images
REPOSITORY             TAG             IMAGE ID            CREATED             SIZE
exampleapp             latest          83wse0edc28a        2 seconds ago       153MB
python                 3.6             05sob8636w3f        6 weeks ago         153MB
```
After that, it is required to take the application image to the repository.


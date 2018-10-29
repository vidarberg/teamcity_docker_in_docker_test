# Build Docker images in a Docker container
Running the build-system TeamCity in Docker is very easy to set up. However, to build Docker images inside a TeamCity agent running in Docker, a few tweaks need to be put in place. In this test, I'm trying out "fake" Docker-in-Docker by mounting the docker.sock file of the host system.

This project tries out building a clean ASP.NET Core 2.1 Docker project created with the wizard in Visual Studio Community 2017 (15.8.7).

## Set up TeamCity server and an agent in Docker
See [this](https://github.com/vidarberg/teamcity_docker_in_docker_test/docs/docker-compose.yml) for the configuration I used to start one TeamCity server and one agent.

Note the volumes for the agent - especially the "/var/run/docker.sock:/var/run/docker.sock". This makes the Docker host system available to the agent.

To start it:

'''bash
docker-compose up
'''

When it's up, navigate your favourite browser to http://localhost:8111 and step through the questions asked.

## Setting up a project
When I had the TeamCity agent up, I clicked the create new project button, and selected GitHub. I went through the steps of creating a connection and authorizing my user, and finally selected (this) project teamcity_docker_in_docker_test.

After that, TeamCity suggested a few build steps for me. I selected the Docker file, gave my project a good name and tag (docker-in-docker:%build.number%) and clicked Run.

### One tweak
The build didn't work straight out of the box. I had to set the context path to "./" and the path to file to "./DockerInDocker/Dockerfile".

## Pushing to a registry
With the final tweak in place, TeamCity builds the Docker image for my example project. I added one more build step to publish the image to my local registry as well.

To get pushing to work, I adjusted the name I gave the image in the build step to myregistry.localdomain:5000/docker-in-docker:%build.number% and used the same name in the second build step. 

(btw, https://myregistry.localdomain:5000 is my local Docker registry)

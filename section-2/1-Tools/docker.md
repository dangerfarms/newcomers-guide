#Docker

Docker allows you to package an application with all of its dependencies into a standardized unit for software development. More details: https://www.docker.com/what-docker

By using docker we can be sure that the development environment matches the production environment, including all dependencies, etc.

We use docker in every project, so please install it now.

##Install docker-engine

To set up docker go to: https://docs.docker.com/engine/installation/ and follow the setup steps for your system closely.

Creating a docker group:
>  **Note** **For Linux** make sure that you follow the "Create a Docker group" part after the installation of the docker engine. This will enable you to use docker as an unprivileged user.   
Ubuntu: https://docs.docker.com/engine/installation/ubuntulinux/#create-a-docker-group  
Fedora: https://docs.docker.com/engine/installation/fedora/#create-a-docker-group  
For other distros please find it in the installation page.

##Install docker-compose
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a Compose file to configure your application’s services. Then, using a single command, you create and start all the services from your configuration. For more details see: https://docs.docker.com/compose/

Please follow the installation instructions on the link above.

## Mac

[Docker Toolbox](https://www.docker.com/products/docker-toolbox) will get you up and running with everything you need (including `docker-compose`). By using Docker Toolbox, you will be running Docker inside a VM (`docker-machine`). This works well, but it can be slow. 

A better option is [Docker for Mac](https://beta.docker.com/). However, this is still in private beta, and has some minor issues. If you can get a copy, it is much faster and simpler. 

When Docker for Mac is no longer invite-only, we will **always** recommend it over Docker Toolbox.

##Practice
Please take some time now and look through the docs for `docker` and `docker-compose` to familiarize yourself with these tools.

Feel free to ask any questions.

If you already have access to one of our projects, look at the scripts that interact with docker. You can find them in the root folder and the file names usually start with a `.` (dot).

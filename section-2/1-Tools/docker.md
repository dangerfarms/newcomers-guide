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

Use [Docker for Mac](https://docs.docker.com/docker-for-mac/) :)

### Common Issues with Docker for Mac

**docker for mac goes into infinite `starting...` state**
If this happens, first use the diagnosis tool provided to check for issues. If however everything looks OK then you may have a stale data folder. If so follow these steps:

1. Stop/Quit Docker for Mac
2. Uninstall Docker.app from /Applications
3. Run this uninstall script to clean any other lingering docker apps https://raw.githubusercontent.com/docker/toolbox/master/osx/uninstall.sh
4. Run `rm -rf ~/Library/Containers/com.docker.*` to remove the data folder
5. Install as per the Docker for Mac guide.



##Practice
Please take some time now and look through the docs for `docker` and `docker-compose` to familiarize yourself with these tools.

Feel free to ask any questions.

If you already have access to one of our projects, look at the scripts that interact with docker. You can find them in the root folder and the file names usually start with a `.` (dot).

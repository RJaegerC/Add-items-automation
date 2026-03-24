## 📌 Description

The "Add Items" automation is a small project built in JavaScript, running on a Docker container. 
In summary, it allows inputs from a user who can add as many items as they want. 
The items the user adds are stored in a list and persisted using SQLite.

## 🚀 Automation

The focus of the project is, however, building pipelines that can be used to automate the deployment of the project
with its different versions as the application evolves, 
in the Docker Hub environment, using an continuous deployment (CD) pipeline with Jenkins.

Additionally, as different versions of the project will result in changes to the source code, a pipeline is built using GitHub Actions. 
This pipeline includes a set of plugins that scan for any vulnerabilities or other static code analysis issues. 
This pipeline is built to ensure the automatic verification of the source code every time it changes, 
intending to minimize vulnerabilities in the code.

## 📝 Tecnology

- Jenkins
- Github Actions
- Javascript
- SQLite
- Docker

## ⚙️ Setup for jenkins 

-cd to jenkins directory

-run:

**docker build -t myjenkins-blueocean:2.414.2 .**

- Create the jenkins separate network for security purposes:

**docker network create jenkins**

- Run the container with the jenkins master server:

**docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.414.2**

- get the password:

**docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword**

- first connection:

**https://localhost:8080/**

- alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine (this gonna be used for the jenkins agent server):

**docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
docker inspect <container_id> | grep IPAddress**

## ⚙️ The js application

- It can run on your local machine with:

**$ yarn install** and **$ yarn start**

- Or by dockerfile in the root directory:

**docker build -t myapp .** and **docker run -p 3000:3000 myapp**

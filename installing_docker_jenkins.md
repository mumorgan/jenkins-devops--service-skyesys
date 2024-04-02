# Running Jenkins on Docker
Full instructions here: https://www.jenkins.io/doc/book/installing/docker/

### I. Create bridge network
`(sudo) docker network create jenkins`

### II. Download and run docker:dind
This is so we can execute Docker commands inside Jenkins nodes.

`(sudo) docker run \
    --name skyesys-jenkins-docker \
    --rm \
    --detach \
    --privileged \
    --network jenkins \
    --network-alias docker \
    --env DOCKER_TLS_CERTDIR=/certs \
    --volume skyesys-jenkins-docker-certs:/certs/client \
    --volume jenkins-data:/var/jenkins_home \
    --publish 2376:2376 \
    docker:dind \
    --storage-driver overlay2
`

### III. Customise official Jenkins Docker
1. Create Dockerfile with the following content (see Dockerfile.new):
`FROM jenkins/jenkins:jdk11
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"`

2. Build image from this Dockerfile and assign it a meaningful name, for example:
`(sudo) docker build -t skyesys-jenkins-blueocean-jdk11:0.0.1 .`

### IV. Run your image (built in III) as container
Use the following command:
`(sudo) docker run \
    --name skyesys-jenkins-blueocean \
    --restart=on-failure \
    --detach \
    --network jenkins \
    --env DOCKER_HOST=tcp://docker:2376 \
    --env DOCKER_CERT_PATH=/certs/client \
    --env DOCKER_TLS_VERIFY=1 \
    --publish 8081:8080 \
    --publish 50000:50000 \
    --volume jenkins-data:/var/jenkins_home \
    --volume jenkins-docker-certs:/certs/client:ro \
    skyesys-jenkins-blueocean-jdk11:0.0.1`
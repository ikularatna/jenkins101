<h1>Installation</h1>
<br>
<h4>Build the Jenkins BlueOcean Docker Image (or pull and use the one I built)</h4>
<code>  docker build -t myjenkins-blueocean:2.414.2 .
  #IF you are having problems building the image yourself, you can pull from my registry (It is version 2.332.3-1 though, the original from the video)
  docker pull devopsjourney1/jenkins-blueocean:2.332.3-1 && docker tag devopsjourney1/jenkins-blueocean:2.332.3-1 myjenkins-blueocean:2.332.3-1
</code>
<h4>
  Create the network 'jenkins'
</h4>
<code> docker network create jenkins </code>
<h3>Run the Container</h3>
<h5>MacOS / Linux</h5>
<code>docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.414.2</code>

  <h5>Windows</h5>
  <code>docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.414.2</code>

  <h3>Get the Password</h3>
  <code>docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword</code>

  <h3>Connect to the Jenkins</h3>
  <code>https://localhost:8080/</code>


  <h3>Installation Reference:</h3>
  https://www.jenkins.io/doc/book/installing/docker/

  <h3>alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine</h3>
  https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin
  <code>docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
docker inspect <container_id> | grep IPAddress</code>

<h3>Using my Jenkins Python Agent</h3>
<code>docker pull devopsjourney1/myjenkinsagents:python</code>

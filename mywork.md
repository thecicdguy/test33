**Setup Jenkins server:**
   * Created GCP VM
   * Installed Java
   * Installed Jenkins
   * Installed Docker
   * Installed Maven
   * Opened firewall for port 8080
   * Added jenkins user to docker group

**Setup Jfrog artifactory**
   * Created a Jfrog cloud free instance
   * Created a new docker repo on the instance
   * Created a new permission "uploader" with deploy access to repos
   * Created a jenkins-uploader and assigned "uploader" permission

**Created Jenkinsfile**
**Created Dockerfile**

**Run the app:**
   * Created a new contanerized GCP VM instance
   * Opened firewall for port 8080
   * Pulled docker container
   * docker run -d -p 8080:8080 shyamchitgopkar.jfrog.io/docker-repo-docker-local/myapp:latest
   * to stop container get the image id with docker ps -a 
   * and then docker stop <image-id>

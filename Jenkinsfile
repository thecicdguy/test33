def artUploadServer = Artifactory.server('jfrogartifactory')

pipeline {
    agent any

  //  tools {
        // Install the Maven version configured as "M3" and add it to the path.
    //    maven "M3"
    //}

    stages {
      /*  stage('Compile') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/thecicdguy/test33.git'
                checkout scm
                // Run Maven on a Unix agent.
                sh "mvn -q clean compile"
            }
        }
        stage('RunTests') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/thecicdguy/test33.git'
                checkout scm

                // Run Maven on a Unix agent.
                sh "mvn -q clean test"
            }
        } */
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "https://shyamchitgopkar.jfrog.io",
                    credentialsId: "jenkins-uploader"
                )
            }
        }
       /* stage('dockbuild') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/thecicdguy/test33.git'
                checkout scm

                // Run Maven on a Unix agent.
                sh "mvn -q -U clean package" 
                sh "docker build --build-arg JAR_FILE=target/*.jar -t myorg/myapp ." 
                sh "docker images"
               // sh "docker push $artUploadServer/docker-repo-docker-local/myorg/myapp:latest"
            }
        }*/

        stage ('Push image to Artifactory') {
            steps {
                rtDockerPush(
                    serverId: "ARTIFACTORY_SERVER",
                    image: "https://shyamchitgopkar.jfrog.io/docker-repo-docker-local/myorg/myapp:latest",
                    // Host:
                    // On OSX: "tcp://127.0.0.1:1234"
                    // On Linux can be omitted or null
                    targetRepo: "docker-repo-docker-local",
                    // Attach custom properties to the published artifacts:
                )
            }
        }
    
        
       /* stage('uploadart') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/thecicdguy/test33.git'
                checkout scm

                // Run Maven on a Unix agent.
                sh "mvn -U clean package" 
                //sh "docker build --build-arg JAR_FILE=target/*.jar -t myorg/myapp ."
                script {
                    echo 'Publishing Artifacts to Artifactory'                               
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "target/*.jar",
                                "target": "docker-repo-docker-local"
                            }
                        ]
                    }"""
                    def buildInfo = artUploadServer.upload(uploadSpec)
                    artUploadServer.publishBuildInfo(buildInfo)
                }            
            }
        }*/
          //  post {
          //      // If Maven was able to run the tests, even if some of the test
          //      // failed, record the test results and archive the jar file.
          //      success {
          //          junit '**/target/surefire-reports/TEST-*.xml'
          //          archiveArtifacts 'target/*.jar'
          //      }
          //  }
      //  }
    }
}

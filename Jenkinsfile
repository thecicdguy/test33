def artUploadServer = Artifactory.server('jfrogartifactory')

pipeline {
    agent any

  //  tools {
        // Install the Maven version configured as "M3" and add it to the path.
    //    maven "M3"
    //}

    stages {
        stage('Compile') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/thecicdguy/test33.git'
                checkout scm

                // Run Maven on a Unix agent.
                sh "mvn clean compile"
            }
        }
        stage('RunTests') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/thecicdguy/test33.git'
                checkout scm

                // Run Maven on a Unix agent.
                sh "mvn clean test"
            }
        }
        
        stage('dockbuild') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/thecicdguy/test33.git'
                checkout scm

                // Run Maven on a Unix agent.
                sh "mvn -U clean package" 
                sh "sudo docker build --build-arg JAR_FILE=target/*.jar -t myorg/myapp ."           
            }
        }
        stage('uploadart') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/thecicdguy/test33.git'
                checkout scm

                // Run Maven on a Unix agent.
                sh "mvn -U clean package" 
                //sh "sudo docker build --build-arg JAR_FILE=target/*.jar -t myorg/myapp ."
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
        }
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

def artUploadServer = Artifactory.server('jfrogartifactory')

pipeline {
    agent any

    environment{
        jfrogcred = credentials('jfroguploader')
    }
    
    stages {
        
        stage ('CheckoutCode') {
            steps {
                checkout scm
            }
            post {
                success {
                    echo "CheckoutCode stage completed"
                }
                failure {
                    echo "CheckoutCode stage failed"
                }
            }
        }
        
        stage('Compile') {
            steps {
                sh "mvn -q clean compile"
            }
            post {
                success {
                    echo "Compile stage completed"
                }
                failure {
                    echo "Compile stage failed"
                }
            }
        }
        
        stage('RunTests') {
            steps {
                sh "mvn -q clean test"
            }
            post {
                success {
                    echo "RunTests stage completed"
                }
                failure {
                    echo "RunTests stage failed"
                }
            }
        } 

        stage('package') {
            steps {
                sh "mvn --quiet -U clean package" 
            }
            post {
                success {
                    echo "package stage completed"
                }
                failure {
                    echo "package stage failed"
                }
            }
        }
        
        stage ('buildTagDocker'){
            steps{
                sh "docker build --build-arg JAR_FILE=target/*.jar -t myapp ."  
                sh "docker images"
                sh "docker tag myapp shyamchitgopkar.jfrog.io/docker-repo-docker-local/myapp:latest"
            }
            post {
                success {
                    echo "buildTagDocker stage completed"
                }
                failure {
                    echo "buildTagDocker stage failed"
                }
            }
        }

        stage ('pushToArtifactory') {
            steps {
                sh "echo $jfrogcred_PSW | docker login shyamchitgopkar.jfrog.io -u $jfrogcred_USR --password-stdin"
                sh "docker push shyamchitgopkar.jfrog.io/docker-repo-docker-local/myapp:latest"
            }
            post {
                success {
                    echo "pushToArtifactory stage completed"
                }
                failure {
                    echo "pushToArtifactory stage failed"
                }
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
    }
    post {
        always {
            deleteDir()
            sh "docker rmi myapp"
        }
        success {
            echo "Job completed successfully"
        }
        failure {
            echo "Job failed"
        }
    }
}

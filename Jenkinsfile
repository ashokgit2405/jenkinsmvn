pipeline {
    agent any
    tools {
        maven 'Maven 3.5.4'
        jdk 'jdk8'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -f hello-app/pom.xml -B -DskipTests clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts....."
                    archiveArtifacts artifacts: '**/*.jar'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -f hello-app/pom.xml test'
            }
            post {
                always {
                    junit 'hello-app/target/surefire-reports/*.xml'
                }
                def server = Artifactory.server "jfrog-demo"

                def uploadSpec =
                       ...{
                        "files": [
                                {
                                     "pattern": "target/*.jar",
                                     "target": "default-maven-local"
                                 }
                           ]
                        }...
                def buildInfo2 =serv.upload spec: uploadSpec

                server.publishBuildInfo buildInfo2
                     }
                    } 
                 }
                    
            }
        }
    }
}

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
                

        // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
        def server = Artifactory.server "jfrog-demo"

        def buildInfo = Artifactory.newBuildInfo()
        // Set custom build name and number.
        buildInfo.setName 'holyFrog'
        buildInfo.setNumber '42'

        // Read the upload spec which was downloaded from github.
        def uploadSpec = readFile 'jenkins-examples/pipeline-examples/resources/recursive-flat-upload.json'
        // Upload to Artifactory.
        server.upload spec: uploadSpec, buildInfo: buildInfo

        // Publish build info.
        server.publishBuildInfo buildInfo
  
                    
            }
        }
    }
}

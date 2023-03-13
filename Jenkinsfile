pipeline {
    agent any
    tools {
        maven 'Maven 3.6.3'
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
    stages {  
      stage ('upload') {
      def server = Artifactory.server "jfrog-demo"
      def buildInfo = Artifactory.newBuildInfo()
      buildInfo.env.capture = true
      buildInfo.env.collect()

      def uploadSpec = """{
        "files": [
          {
            "pattern": "**/target/*.jar",
            "target": "libs-snapshot-local"
          }, {
            "pattern": "**/target/*.pom",
            "target": "libs-snapshot-local"
          }, {
            "pattern": "**/target/*.war",
            "target": "libs-snapshot-local"
          }
        ]
      }"""
      // Upload to Artifactory.
      server.upload spec: uploadSpec, buildInfo: buildInfo

      buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
      // Publish build info.
      server.publishBuildInfo buildInfo
        stage('Test') {
            steps {
                sh 'mvn -f hello-app/pom.xml test'
            }
            post {
                always {
                    junit 'hello-app/target/surefire-reports/*.xml'
                }
            }
        }
    }
  }
 }
} 

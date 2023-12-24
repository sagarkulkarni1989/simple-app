pipeline {
    agent any
    tools {
        maven 'maven3'
    }
   
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                    archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
                }
            }
        }
        stage('Upload War To Nexus') {
            steps {
                nexusArtifactUploader artifacts: [
                 [
                  artifactId: 'simple-app', 
                  classifier: '', 
                  file: 'target/simple-app-3.0.0.war', 
                  type: 'war'
                 ]
               ], 
               credentialsId: 'admin', 
               groupId: 'in.javahome', 
               nexusUrl: '172.31.34.143:8081', 
               nexusVersion: 'nexus3', 
               protocol: 'http', 
              repository: 'http://65.1.95.114:8081/repository/simpleapp-release', 
              version: '3.0.0'
          }
     }
 }
}

pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
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
                script {
                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "simpleapp-snapshot" : "simpleapp-release"

                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app',
                            classifier: '',
                            file: "target/simple-app-${mavenPom.version}.war",
                            type: 'war'
                        ]
                    ],
                    credentialsId: 'admin',
                    groupId: 'in.javahome',
                    nexusUrl: 'http://172.31.34.143:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: nexusRepoName,
                    version: "${mavenPom.version}"
                }
            }
        }
    }
}

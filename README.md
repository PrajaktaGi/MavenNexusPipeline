# MavenNexusPipeline
Launch two servers, maven and nexus
1)	Install apache maven on maven server, and configure nexus on nexus server
2)	Git clone the code myweb in maven server , and try to run mvn clean package command in maven server
3)	If build is success install Jenkins
4)	Create pipelin by adding stepsd – git checkout, maven build, nexus upload for this need to install plugin in Jenkins
5)	And run the pipeline
Pipeline
pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                git 'https://github.com/PrajaktaGi/myweb.git'
            }
        }
        
        stage('maven build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Nexus upload') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'myweb', classifier: '', file: 'target/myweb-8.2.12.war', type: 'war']], credentialsId: 'nexus3', groupId: 'in.javahome', nexusUrl: '172.31.3.142:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-releases', version: '8.2.12'
            }
        }
    }
}

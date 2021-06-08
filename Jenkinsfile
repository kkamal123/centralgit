pipeline {
    agent any
    environment {
    registry = "registry.hub.docker.com/kkamal/docker-test1"
    registryCredential = 'dockerhub'
    }
    
    stages {
        
        stage('Cloning Git') {
            steps {
          //      git branch: 'master', credentialsId: 'github', url: 'https://github.com/kkamal123/centralgit.git'
                  sh 'git clone https://github.com/kkamal123/centralgit.git'
            }
        }
        
        stage('Building image') {
            steps{
                script {
                    sh 'cd /var/jenkins_home/workspace/docker-test/centralgit'
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        
        stage('Push the docker image to dockerhub') {
            steps{    
                script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                    }
                }
            }   
        }
        
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}

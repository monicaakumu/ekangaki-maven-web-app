pipeline {
    agent any
    
    tools {
       maven 'Maven3'
    }

    stages {
        stage('Hello Africa') {
            steps {
                echo 'My first Declarative script'
            }
        }
        stage('Code Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Ekangaki/ekangaki-maven-web-app.git'
            }
        }
        stage('Main Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker image') {
            steps {
                sh 'docker build -t ekangaki/maven-web-application:${BUILD_NUMBER} .'
            }
        }
        stage('Build Docker Container') {
            steps {
                sh 'docker run -it -d -p 9091:8080 ekangaki/maven-web-application:${BUILD_NUMBER}'
            }
        }
        stage('Push Docker image to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'Dockerhubcredentials', variable: 'dockerhubcredentials')]) {

                   sh "docker login -u ekangaki -p ${dockerhubcredentials}"
                        sh "docker push ekangaki/maven-web-application:${BUILD_NUMBER}"
                        
                    }
                        
                    }
                }
            }
        
    }
}

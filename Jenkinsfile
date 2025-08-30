pipeline {
    agent any
    
    tools {
        maven 'M3'    // must match the Maven installation name in Jenkins
        jdk   'jdk17' // must match the JDK installation name in Jenkins
    }

    stages {
        stage('Hello Africa') {
            steps {
                echo 'My first Declarative script'
            }
        }

        stage('Code Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/monicaakumu/ekangaki-maven-web-app.git'
            }
        }

        stage('Maven Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Maven Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Main Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t moniakumu/maven-web-application:${BUILD_NUMBER} .'
            }
        }

        stage('Build Docker Container') {
            steps {
                sh 'docker run -d -p 8085:8080 moniakumu/maven-web-application:${BUILD_NUMBER}'
            }
        }

        stage('Push Docker image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker') {
                        sh "docker push moniakumu/maven-web-application:${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}

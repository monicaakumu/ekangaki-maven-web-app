pipeline {
    agent any

    tools {
        maven 'M3'
        jdk   'jdk17'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
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

        stage('Sonar Scanner') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=maven-web-application \
                        -Dsonar.host.url=http://52.23.169.111:9000 \
                        -Dsonar.login=squ_dcb8613eb894423cd6fc41c728a8e6e0ea179c74
                    """
                }
            }
        }

        stage('Main Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Upload Artifact to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '52.23.169.111:8081',
                    groupId: 'com.mt',
                    version: '0.0.2-RELEASE',
                    repository: 'Jenkins-Release-Repository',
                    credentialsId: 'nexus-credential',
                    artifacts: [[
                        artifactId: 'maven-web-application',
                        classifier: '',
                        file: 'target/maven-web-application.war',
                        type: 'war'
                    ]]
                )
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t moniakumu/maven-web-application:${BUILD_NUMBER} .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8085:8080 moniakumu/maven-web-application:${BUILD_NUMBER}'
            }
        }

        stage('Push Docker image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-credential') {
                        sh "docker push moniakumu/maven-web-application:${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}


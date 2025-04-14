pipeline {
    agent any
    environment {
        IMAGE_NAME = "marijaklisarovska/myjenkins-blueocean:2.492"
    }
    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }
        stage('Build image') {
            steps {
                script {
                    app = docker.build("${IMAGE_NAME}")
                }
            }
        }
        stage('Push image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-kiii') {
                        app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        app.push("${env.BRANCH_NAME}-latest")
                        // signal the orchestrator that there is a new version
                    }
                }
            }
        }
    }
}

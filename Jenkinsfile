pipeline {
    agent any

    tools {
        maven 'Maven_3.9.11'
        jdk 'JDK17'
    }

    environment {
        DOCKER_USER = 'kumuthini2026'
        DOCKER_PASS = 'Asha@2026'
        IMAGE_NAME = "${DOCKER_USER}/test:todoimg"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/kumuthiniak/COMP94.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Docker Build & Push') {
            steps {
                bat "docker build -t ${IMAGE_NAME} ."
                bat "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
                bat "docker push ${IMAGE_NAME}"
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace..."
            deleteDir()
        }
        success {
            echo "Build, Test, and Docker Push completed successfully!"
        }
        failure {
            echo "Build failed. Check logs for details."
        }
    }
}

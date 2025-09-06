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
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_LOGIN = 'sqa_4259bd8980efee0a86c8fb4ce215e00e0427fc1c'
    }

    triggers {
        pollSCM('H/15 * * * *')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/kumuthiniak/COMP94.git'
            }
        }

        stage('Build') {
            steps {
                // Run Maven from the root folder where pom.xml exists
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                bat "mvn sonar:sonar -Dsonar.projectKey=devops-integration -Dsonar.host.url=${SONAR_HOST_URL} -Dsonar.login=${SONAR_LOGIN}"
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
            echo "Build, Test, SonarQube Analysis, and Docker Push completed successfully!"
        }
        failure {
            echo "Build failed. Check logs for details."
        }
    }
}

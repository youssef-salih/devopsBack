pipeline {
    agent any

    environment {
        JAVA_HOME = "C:\\Program Files\\Java\\jdk-25"
        PATH = "${env.JAVA_HOME}\\bin;C:\\apache-maven-3.9.8\\bin;C:\\Program Files\\Docker\\Docker\\resources\\bin;${env.PATH}"
        IMAGE_NAME = "springback"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/youssef-salih/devopsBack.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Docker Run') {
            steps {
                bat '''
                docker stop %IMAGE_NAME% || echo "No container running"
                docker rm %IMAGE_NAME% || echo "No container to remove"
                docker run -d -p 8080:8080 --name %IMAGE_NAME% %IMAGE_NAME%
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Spring app built and running in Docker!'
        }
        failure {
            echo '❌ Build or Docker failed!'
        }
    }
}

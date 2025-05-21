pipeline {
    agent any

    environment {
        DOCKER_HOST = 'unix:///var/run/docker.sock'
        APP_NAME = 'jenkins'
        CONTAINER_NAME = 'laravel-running'
        PORT = '8000'
    }

    stages {
        stage('Clone Source') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/agthra/jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                // FIX: Pakai env.DOCKER_HOST jika mau ditampilkan
                echo "${env.DOCKER_HOST}"
                sh "unset DOCKER_HOST"
                echo $DOCKER_HOST
                sh "docker build -t ${env.APP_NAME} ."
            }
        }

        stage('Stop Old Container') {
            steps {
                echo 'Stopping and removing old container (if exists)...'
                sh "docker rm -f ${env.CONTAINER_NAME} || true"
            }
        }

        stage('Run New Container') {
            steps {
                echo 'Running new container...'
                sh "docker run -d -p ${env.PORT}:8000 --name ${env.CONTAINER_NAME} ${env.APP_NAME}"
            }
        }
    }

    post {
        success {
            script {
                echo "Deployment successful. Visit http://<ip-server>:${env.PORT}"
            }
        }
        failure {
            echo "Something went wrong!"
        }
    }
}

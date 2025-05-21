pipeline {
    agent any

    environment {
        APP_NAME = 'laravel-app'
        CONTAINER_NAME = 'laravel-running'
        PORT = '8000'
    }

    stages {
        stage('Clone Source') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/gengtzy/laravel.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t $APP_NAME ."
            }
        }

        stage('Stop Old Container') {
            steps {
                echo 'Stopping and removing old container (if exists)...'
                sh "docker rm -f $CONTAINER_NAME || true"
            }
        }

        stage('Run New Container') {
            steps {
                echo 'Running new container...'
                sh "docker run -d -p $PORT:8000 --name $CONTAINER_NAME $APP_NAME"
            }
        }
    }

    post {
        success {
            echo "Deployment successful. Visit http://<ip-server>:$PORT"
        }
        failure {
            echo "Something went wrong!"
        }
    }
}

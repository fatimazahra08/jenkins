pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                bat 'docker build -t my-python-app .'
            }
        }

        

        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing the Docker image to Docker Hub...'
                bat '''
                    docker login -u %DOCKER_HUB_CREDENTIALS_USR% -p %DOCKER_HUB_CREDENTIALS_PSW%
                    docker tag my-python-app %DOCKER_HUB_CREDENTIALS_USR%/my-python-app:latest
                    docker push %DOCKER_HUB_CREDENTIALS_USR%/my-python-app:latest
                '''
            }
        }

        stage('Deploy') {
    steps {
        echo 'Deploying the application locally...'
        bat '''
            docker stop my-python-app || exit 0
            docker rm my-python-app || exit 0
            docker run -d -p 5000:5000 --name my-python-app %DOCKER_HUB_CREDENTIALS_USR%/my-python-app:latest
        '''
    }
}

        }
    
}

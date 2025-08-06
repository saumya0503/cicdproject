pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "saumya0503/cicdproject"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

      

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% ."
            }
        }

      stage('Push to DockerHub') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'saumya0503',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS')]) {

            bat '''
            echo Logging into DockerHub...
            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
            docker push saumya0503/cicdproject:latest
            '''
        }
    }
}


        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
            }
        }
    }
}

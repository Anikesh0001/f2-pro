pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "aishwaryagirish/my-python-app1"
        DOCKER_TAG = "latest"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh '/usr/local/bin/docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'Credential',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | /usr/local/bin/docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh '/usr/local/bin/docker push $DOCKER_IMAGE:$DOCKER_TAG'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                /usr/local/bin/docker stop myapp-container || true
                /usr/local/bin/docker rm myapp-container || true
                /usr/local/bin/docker run -d -p 5001:5000 --name myapp-container $DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
    }
}

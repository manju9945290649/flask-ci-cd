pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
        CONTAINER_NAME = "my-container"
        PORT = "8081"
    }

    stages {

        stage('Build Image') {
            steps {
                sh 'podman build --format docker -t $IMAGE_NAME .'
                */sh 'podman build -t $IMAGE_NAME .'*/
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                podman stop $CONTAINER_NAME || true
                podman rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                podman run -d -p $PORT:5000 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }

        stage('Test API') {
            steps {
                sh 'curl http://localhost:$PORT/hello'
            }
        }
    }
}

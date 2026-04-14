pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
        CONTAINER_NAME = "my-container"
        PORT = "8082"
    }

    stages {

        stage('Build Image') {
            steps {
                sh 'podman build --format docker -t $IMAGE_NAME .'
                sh 'podman build -t $IMAGE_NAME .'
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
        podman rm -f $CONTAINER_NAME || true
        fuser -k 8081/tcp || true
        podman run -d -p $PORT:5000 --name $CONTAINER_NAME $IMAGE_NAME
        '''
    }
}

        stage('Test API') {
            steps {
                sleep 5
                sh 'curl http://localhost:$PORT/hello'
            }
        }
    }
}

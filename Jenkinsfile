pipeline {
    agent any

    environment {
        IMAGE_NAME = "smart-study-planner"
        CONTAINER_NAME = "studyplanner"

        EC2_HOST = "54.82.76.213"
        EC2_USER = "ubuntu"

        SSH_CREDENTIALS = "smart-planner"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Aakarsh7887/smart-study-planner.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Run Health Validation') {
            steps {
                sh '''
                docker rm -f temp-container || true

                docker run -d \
                --name temp-container \
                -p 8090:80 \
                $IMAGE_NAME

                echo "Waiting for container startup..."

                sleep 10

                curl -f http://localhost:8090

                docker stop temp-container
                docker rm temp-container
                '''
            }
        }

        stage('Deploy to EC2') {
            steps {

                sshagent(credentials: ["${SSH_CREDENTIALS}"]) {

                    sh '''
                    ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_HOST "

                    docker stop $CONTAINER_NAME || true

                    docker rm $CONTAINER_NAME || true

                    docker rmi $IMAGE_NAME || true
                    "
                    '''

                    sh '''
                    docker save $IMAGE_NAME | gzip | \
                    ssh -o StrictHostKeyChecking=no \
                    $EC2_USER@$EC2_HOST \
                    'gunzip | docker load'
                    '''

                    sh '''
                    ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_HOST "

                    docker run -d \
                    --name $CONTAINER_NAME \
                    -p 80:80 \
                    --restart always \
                    $IMAGE_NAME
                    "
                    '''
                }
            }
        }

        stage('Deployment Verification') {
            steps {

                sh '''
                echo "Checking deployed website..."

                sleep 10

                curl -f http://$EC2_HOST
                '''
            }
        }
    }

    post {

        success {
            echo '===================================='
            echo 'Deployment Successful!'
            echo '===================================='
        }

        failure {
            echo '===================================='
            echo 'Pipeline Failed!'
            echo '===================================='
        }

        always {
            sh 'docker system prune -f || true'
        }
    }
}

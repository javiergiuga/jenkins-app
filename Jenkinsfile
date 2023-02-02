pipeline {
    agent any
    environment {
        EC2INSTANCES = 'ec2-user@3.87.67.206'
        APPNAME = 'python-demo'
        REGISTRY = 'javiergiuga'
        DOCKER_HUB_LOGIN = credentials('docker-hub')
    }

    stages {
        stage('Init') {
            steps {
                echo 'Stage Init'
            }
        }
    stage('Test') {
            steps {
                echo 'Stage Test'
            }
        }
    stage('Build') {
            steps {
                echo 'Stage Build'
                sh 'docker build -t ${APPNAME}/${BUILD_NUMBER} .'
                sh 'docker tag ${APPNAME}/${BUILD_NUMBER} ${REGISTRY}/${APPNAME}/:${BUILD_NUMBER}'
            }
        }
    stage('Push to Registry') {
            steps {
                echo 'Stage Push'
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --pasword=$DOCKER_HUB_LOGIN_PWS'
                sh 'docker push ${REGISTRY}/${APPNAME}/:${BUILD_NUMBER}'
            }
        }
    stage('Deploy') {
            steps {
                echo 'Stage Deploy'
                sh 'touch prueba.txt' 
                sshagent(['ssh-ec2']){
                 sh 'scp -o StrictHostKeyChecking=no prueba.txt ${EC2INSTANCES}:/home/ec2-user'   
                }
            }
        }
    stage('Notification') {
            steps {
                echo 'Stage Notify'
            }
        }
    }
}
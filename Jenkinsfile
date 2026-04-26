pipeline {
    agent any

    environment {
        IMAGE = "yourdockerhub/python-cicd"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/blaxmankumar/python-cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop python-cicd || true
                docker rm python-cicd || true
                docker pull $IMAGE
                docker run -d -p 5000:5000 --name python-cicd $IMAGE
                '''
            }
        }
    }
}

pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git branch: 'master',
                url: 'https://gitlab.com/bouaziziazza45/datacamp_docker_angular.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --legacy-peer-deps'
            }
        }

        stage('Build Angular') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t angular-app .'
            }
        }

        stage('Deploy sur VM') {
            steps {
                sh '''
                ssh vboxuser@192.168.56.102 "
                docker stop angular-container || true &&
                docker rm angular-container || true &&
                docker run -d --name angular-container -p 8085:80 angular-app
                "
                '''
            }
        }
    }
}

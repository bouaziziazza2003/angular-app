pipeline {
    agent any
    stages {
        stage('Clone Stage') {
            steps {
                echo "Code clone depuis GitLab!"
            }
        }
        stage('Get Version') {
            steps {
                script {
                    DOCKER_TAG = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    echo "Docker tag: ${DOCKER_TAG}"
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t azza9292/angular-app:${DOCKER_TAG} ."
            }
        }
        stage('DockerHub Push') {
            steps {
                withCredentials([string(credentialsId: 'mydockerhubpassword', variable: 'DockerHubPassword')]) {
                    sh "docker login -u azza9292 -p ${DockerHubPassword}"
                    sh "docker push azza9292/angular-app:${DOCKER_TAG}"
                }
            }
        }
        stage('Deploy') {
            steps {
                sh "docker stop angular-container 2>/dev/null || true"
                sh "docker rm angular-container 2>/dev/null || true"
                sh "docker run -d --name angular-container -p 8085:80 azza9292/angular-app:${DOCKER_TAG}"
                echo "Deploye sur http://localhost:8085"
            }
        }
    }
}

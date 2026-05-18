pipeline {
    agent any
    stages {
        stage('Clone Stage') {
            steps {
                echo "Code cloné depuis GitHub!"
            }
        }
        stage('Prepare Dockerfile') {
            steps {
                sh '''cat > Dockerfile << 'EOF'
### STAGE 1: Build ###
FROM node:12.7-alpine AS build
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build
### STAGE 2: Run ###
FROM nginx:1.17.1-alpine
COPY nginx.conf /etc/nginx/nginx.conf
COPY --from=build /usr/src/app/dist/aston-villa-app /usr/share/nginx/html
EOF'''
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
                }
                sh "docker push azza9292/angular-app:${DOCKER_TAG}"
            }
        }
    }
}

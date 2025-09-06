pipeline {
    agent any

    environment {
        // Maven tool configured in Jenkins Global Tools
        MVN_HOME = tool 'maven-3.5.2'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/shankutanna/DevOps-java-container.git'
            }
        }

        stage('Build Project with Maven') {
            steps {
                sh "'${MVN_HOME}/bin/mvn' clean install"
            }
        }

        stage('Build Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds',
                                                  usernameVariable: 'DOCKER_USER',
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                      docker pull openjdk:8-jdk-alpine || true
                      docker build -t $DOCKER_USER/devopsexample:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds',
                                                  usernameVariable: 'DOCKER_USER',
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                      docker push $DOCKER_USER/devopsexample:${BUILD_NUMBER}
                    '''
                }
            }
        }
    }
}

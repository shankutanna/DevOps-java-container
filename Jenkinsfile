node {
    def mvnHome = tool 'maven-3.5.2'
    def dockerImage
    def dockerImageTag = "devopsexample:${env.BUILD_NUMBER}"

    stage('Clone Repo') {
        git 'https://github.com/shankutanna/DevOps-java-container.git'
        mvnHome = tool 'maven-3.5.2'
    }    

    stage('Build Project') {
        sh "'${mvnHome}/bin/mvn' clean install"
    }
		
    stage('Build Docker Image') {
        dockerImage = docker.build(dockerImageTag)
    }
   	  
    stage('Docker Login & Push') {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
            
            // Tag with your Docker Hub repo name
            sh "docker tag ${dockerImage.imageName()} $DOCKER_USER/devopsexample:${env.BUILD_NUMBER}"
            
            // Push to Docker Hub
            sh "docker push $DOCKER_USER/devopsexample:${env.BUILD_NUMBER}"
        }
    }
}


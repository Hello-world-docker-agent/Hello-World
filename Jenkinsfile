node ('docker-agent') {
    
    def buildNumber = BUILD_NUMBER
    def mavenHome= tool name: "maven", type: "maven"
    
    stage ("Git clone") {
        
        git branch: 'develop', url: 'https://github.com/Hello-world-docker-agent/Hello-World.git'
    }
    
    stage("Maven Package") {
        
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage("Creation of Docker Image") {
        
        sh "docker build -t geethika609/hello-world-dev:${buildNumber} ."
    }
    
    stage("Docker login and push") {
        
        withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'DockerHubPwd')]) {
            sh "docker login -u geethika609 -p ${DockerHubPwd}"
        }
        sh "docker push geethika609/hello-world-dev:${buildNumber}"
    }
    
    stage("Deploy Application as Docker container in Docker Deployment Server") {
        
        sshagent(['Docker_Server_SSH']) {
            
            sh "ssh -o StrictHostKeyChecking=no ubuntu@52.66.211.183 docker rm -f helloworlddev || true"
            sh "ssh -o StrictHostKeyChecking=no ubuntu@52.66.211.183 docker run -d -p 9090:8080 --name helloworlddev geethika609/hello-world-dev:${buildNumber}"
        }
    }
    
}

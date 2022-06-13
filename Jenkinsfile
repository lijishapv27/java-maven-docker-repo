pipeline {
    agent any
 
    stages {
        stage('Docker Build and Tag') {
            steps {
              
                sh 'docker build -t nginxtest:latest .' 
                sh 'docker tag nginxtest nikhilnidhi/nginxtest:latest'
                sh 'docker tag nginxtest nikhilnidhi/nginxtest:$BUILD_NUMBER'
            }
        }
     
        stage('Publish image to Docker Hub') {
            steps {
                withDockerRegistry([ credentialsId: "docker-credential", url: "" ]) {
                    sh  'docker push lijisha27/java-maven-jenkins:latest'
                    sh  'docker push java-maven-docker-repo lijisha27/java-maven-jenkins:$BUILD_NUMBER' 
                 }
                  
            }
        }
     
        stage('Run Docker container on Jenkins Agent') {
             
            steps {
                sh "docker run -d -p 4030:80 lijisha27/java-maven-jenkins"
 
            }
        }
        
        stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 4001:80 lijisha27/java-maven-jenkins"
 
            }
        }
    }
}
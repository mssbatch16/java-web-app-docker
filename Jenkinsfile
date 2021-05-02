node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
         withMaven(jdk: 'javapath', maven: 'maven') {
         sh "mvn clean package"
        }
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t mytag .'
    }
    
    stage('Push Docker Image'){
       withDockerRegistry([credentialsId: 'faa6d20a-5bc6-4b5e-835e-7e5116e9b1fc']) {
           sh 'docker push mytag:latest'
        }
       
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app dockerhandson/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.20.72 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.20.72 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.20.72 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.20.72 ${dockerRun}"
       }
       
    }
     
     
}

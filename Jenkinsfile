pipeline {
    agent any
     environment {
        registry = "617977097000.dkr.ecr.us-east-1.amazonaws.com/domo_ecr_repo"
    }
   
    stages {
          stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/shyamdev09/jenkins-ECR.git'
            }
        }
           stage('Building image') {
             steps{
                  script {
                   dockerImage = docker.build registry
                   }
      }
           }
    
            stage('Pushing to ECR') {
             steps{  
                  script {
withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {            
            sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 617977097000.dkr.ecr.us-east-1.amazonaws.com'
            sh 'docker push 617977097000.dkr.ecr.us-east-1.amazonaws.com/domo_ecr_repo:latest'
}

}
                  }
            }
             stage('stop previous containers') {
               steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
            stage('Docker Run') {
              steps{
                   script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer 617977097000.dkr.ecr.us-east-1.amazonaws.com/domo_ecr_repo:latest'     
      }
    }
        }
    }
  }

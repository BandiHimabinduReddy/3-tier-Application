pipeline{
    agent any
    environment {
        registry = "180009188669.dkr.ecr.us-east-1.amazonaws.com/jenkins"
    }
    stages{
        stage(" git check_out "){
            steps{
                git branch: "main" , url: "https://github.com/BandiHimabinduReddy/3-tier-Application.git" 
            }
        }
       
        stage('Docker Build') {
             steps{
                  script {
                   dockerImage = docker.build registry
                    }
                }
            }
        stage('Pushing to ECR') {
             steps{  
                  script {
               withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
               credentialsId: 'aws_cred', 
               accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
               secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 180009188669.dkr.ecr.us-east-1.amazonaws.com'
    sh 'docker push 180009188669.dkr.ecr.us-east-1.amazonaws.com/jenkins'
                 }
                    }
                }
        }
       
        stage('Start container') {
          steps {
            sh 'docker compose up -d --no-color --wait'
            sh 'docker compose ps'
        }
    }
        stage("Deploy"){
            steps{
               sh "docker-compose down && docker-compose up -d"
                echo "Deployed to ec2"
            }
        }
    }
}






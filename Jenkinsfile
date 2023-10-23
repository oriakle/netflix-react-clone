pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("public.ecr.aws/y2u4u4u9/netflixoct")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html public.ecr.aws/y2u4u4u9/netflixoct:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://public.ecr.aws/y2u4u4u9/netflixoct', 'ecr:eastus:public.ecr.aws/y2u4u4u9/netflixoct') {
                    // build image
                    def myImage = docker.build("public.ecr.aws/y2u4u4u9/netflixoct")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}

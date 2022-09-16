pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Hemajo/java.git']]])



               sh "mvn clean install"
            }
        
        }
        
        stage('Docker') {
            steps {
                sh "docker build -t pipeline3 ."
            }
        }
        stage('Push to ecr') {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 307815379384.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker tag pipeline3:latest 307815379384.dkr.ecr.us-east-1.amazonaws.com/pipeline3:latest"
                    sh "docker push 307815379384.dkr.ecr.us-east-1.amazonaws.com/pipeline3:latest"
        }
      }
    }



       stage("Deploy to EKS"){
            steps{
                sh 'aws eks update-kubeconfig --name my-cluster --region us-east-1'
              sh '/home/ubuntu/bin/kubectl apply -f deployment.yml'
        }
    }
  }
}

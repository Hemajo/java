pipeline{
  agent any
  stages{
		stage("Maven Build"){
        steps{
            sh "mvn -B -DskipTests clean package"
            sh "mv target/*.war target/myweb.war"
             }
            }
		stage("Docker Build"){
		steps{
			script{
				app = docker.build("ecr-pipeline:${env.BUILD_NUMBER}")
                }
             }
          }
		stage("Docker Push"){
		steps{
			script{
				docker.withRegistry('https://540179555644.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-cred') {
				app.push("${env.BUILD_NUMBER}")
        }
      }
    }
    }
	  stage('Deploy'){
            steps {
			withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
    credentialsId: 'aws-cred',
    accessKeyVariable: 'AKIAX3RJV4U6KHNM6P5R',
    secretKeyVariable: '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']]) {
    			sh 'aws eks update-kubeconfig --region us-east-1 --name java-login-app'
                 sh '/root/bin/kubectl apply -f deployment.yml'
				}
				 
            }
		}
    }
}

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
				app = docker.build("pipeline3:${env.BUILD_NUMBER}")
                }
             }
          }
		stage("Docker Push"){
		steps{
			script{
				docker.withRegistry('https://540179555644.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:ap-south-1:aws-ecr-credentials') {
				app.push("${env.BUILD_NUMBER}")
        }
      }
    }
    }
	  stage('Deploy'){
            steps {
			withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
    credentialsId: 'aws-ecr-credentials',
    accessKeyVariable: 'AKIAX3RJV4U6HZL7AJRA',
    secretKeyVariable: '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']]) {
    			sh 'aws eks update-kubeconfig --region ap-south-1 --name my-cluster'
                 sh 'usr/local/bin/kubectl apply -f deployment.yml'
				}
				 
            }
		}
    }
}

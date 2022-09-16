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
				docker.withRegistry('https://083986437940.dkr.ecr.ap-southeast-1.amazonaws.com', 'ecr:ap-southeast-1:user1-cred') {
				app.push("${env.BUILD_NUMBER}")
        }
      }
    }
    }
  stage('Deploy'){
            steps {
			withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
    credentialsId: 'user1-cred',
    accessKeyVariable: '',
    secretKeyVariable: '']]) {
    			sh 'aws eks update-kubeconfig --region ap-southeast-1 --name eks-cluster'
                 sh 'usr/local/bin/kubectl apply -f deployment.yml'
				}
	    }
  }
  }
}

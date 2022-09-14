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
				app = docker.build("java-web-app:${env.BUILD_NUMBER}")
                }
             }
          }
		stage("Docker Push"){
		steps{
			script{
				docker.withRegistry('https://540179555644.dkr.ecr.ap-south-1.amazonaws.com/pipeline3', 'ecr:us-east-1:aws-ecr-credentials') {
				app.push("${env.BUILD_NUMBER}")
        }
      }
    }
    }
  }
}

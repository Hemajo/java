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
				docker.withRegistry('https://307815379384.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-cred') {
				app.push("${env.BUILD_NUMBER}")
        }
      }
    }
    }
  }
}

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
				docker.withRegistry('https://083986437940.dkr.ecr.ap-southeast-1.amazonaws.com', 'ecr:ap-southeast-1:aws-id') {
				app.push("${env.BUILD_NUMBER}")
        }
      }
    }
    }
  stage('Deploy'){
            steps {
			withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
    credentialsId: 'aws-id',
    accessKeyVariable: 'AKIARHDP3K42BBJFFTEY',
    secretKeyVariable: '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']]) {
    			sh 'aws eks update-kubeconfig --region ap-southeast-1 --name my-cluster'
                 sh '/root/bin/kubectl apply -f deployment.yml'
				}

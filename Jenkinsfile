pipeline {
    agent any

    stages {
		stage('checkout'){
		steps{
		git 'https://github.com/jglick/simple-maven-project-with-tests.git'

		}    
	 }
        stage('Build') {
            steps {

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.skip=true clean install"
  
            }
        }
		stage('Deploy to Dev'){
		steps {
		sh 'mv target/*.jar target/java.jar'
		sshagent(['tomcat2']) {
			sh 'ssh ubuntu@172.31.40.143 rm -rf /opt/tomcat9/webapps/*.jar'
		    sh 'scp $${WORKSPACE}/target/java.jar ubuntu@172.31.40.143:/opt/tomcat9/webapps/'
		    sh 'ssh ubuntu@172.31.40.143 sudo service tomcat restart'
		}
	}
	}
}
}
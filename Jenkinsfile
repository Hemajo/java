pipeline{

  agent any

  stages{
    stage("Build"){

        steps{

            sh "mvn -B -DskipTests clean package"

            sh "mv target/*.war target/myweb.war"

             }

            }

      stage("Scan"){

        steps{

         withSonarQubeEnv(installationName:'sonar',credentialsId:'sonar') {

         sh "mvn sonar:sonar"

         }

        }

      }

      stage('Test') {

        steps {

        sh(script: 'mvn -Dmaven.test.failure.ignore test')

      }

      post {

        always {

            junit '**/target/surefire-reports/TEST-*.xml'

             }

           }

      }

      stage("deploy"){

       steps{

          deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://65.1.100.125:8090/')], contextPath: 'myapp', war: '**/*.war'

          }

        }

      }

    }

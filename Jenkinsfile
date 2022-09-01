pipeline{

  agent any

  stages{

      stage("Maven Build"){

        steps{

            sh "mvn -B -DskipTests clean package"

            sh "mv target/*.war target/myweb.war"

             }

            }

      stage("deploy"){

       steps{

          deploy adapters: [tomcat8(credentialsId: 'tomcatuser', path: '', url: 'http://13.233.138.142:8090')], contextPath: 'myapp', war: '**/*.war'         

          }

        }

      }

    }

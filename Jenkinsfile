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

          deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://65.0.176.146:8080/')], contextPath: 'myapp', war: '**/*.war'          

          }

        }

      }

    }

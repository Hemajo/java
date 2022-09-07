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

          deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://65.1.100.125:8090/')], contextPath: 'myapp', war: '**/*.war'        

          }

        }

      }

    }

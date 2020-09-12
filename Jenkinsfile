pipeline{
    agent any
    stages{
        stage('Build Backend'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }
          stage('Unit Tests'){
            steps{
                bat 'mvn test'
            }
          }
          
       stage('Deploy Backend'){
            steps{
               deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8081/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
           stage('API Test'){
            steps{
               dir('api-test'){
               git 'https://github.com/daltonromano84/tasks-api-test'
               bat 'mvn test'
               }
            }
        }


          stage('Deploy FrontEnd'){
            steps{
                 dir('frontend'){
                 git 'https://github.com/daltonromano84/tasks-frontend'
                 bat 'mvn clean package'
                 deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8081/')], contextPath: 'tasks', war: 'target/tasks.war'
            }
            }
        }

             stage('Functional Test'){
            steps{
                 dir('functional-test'){
                 git 'https://github.com/daltonromano84/tasks-functional-tests'
                 bat 'mvn test'
                
            }
            }
        }


    }
}
  


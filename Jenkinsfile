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
          stage('Sonar Amalysis'){
                 environment{
                     scannerHome = tool 'SONAR_SCANNER'
                 }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                         bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.99.100:9000 -Dsonar.login=5beb28f1a79f5e76d5c802efca095a4d8261d2f5 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java " 
                }
              

            }
        }
          stage('Quality Gate'){
        steps{
            sleep(20)
            timeout(time:1, unit:'MINUTES'){
            waitForQualityGate abortPipeline:true
            }
        }
    }
       stage('Deploy Backend'){
            steps{
               deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8081/')], contextPath: 'asks-backend', war: 'target/tasks-backend.war'
            }
        }
           stage('API Test'){
            steps{
               git 'https://github.com/daltonromano84/tasks-api-test'
               bat 'mvn test'
            }
        }

    }
}
  



pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage('SonarQube Analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube'
                          }
                   }  
              }
        }
        
        stage('SonarQube Quality Analysis'){
            steps{
                script{
                   waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                   }  
              }
        }
        
        
       stage("Docker Build & Docker Push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 3.34.125.183.199:8083/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 34.125.183.199:8083 
                                docker push  34.125.183.199:8083/springapp:${VERSION}
                                docker rmi 34.125.183.199:8083/springapp:${VERSION}
                            '''
                    }
                }
            }
        }
        
        
       
   }
}

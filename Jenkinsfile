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
        stage("docker build & docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 18.116.29.222:8085/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 18.116.29.222:8085 
                                docker push  18.116.29.222:8085/springapp:${VERSION}
                                docker rmi 18.116.29.222:8085/springapp:${VERSION}
                            '''
                    }
                }
            }
        }
        
       
   }
}

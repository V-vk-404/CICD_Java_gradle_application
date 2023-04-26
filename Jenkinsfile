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
                                docker build -t 3.138.156.34:8085/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 3.138.156.34:8085 
                                docker push  3.138.156.34:8085/springapp:${VERSION}
                                docker rmi 3.138.156.34:8085/springapp:${VERSION}
                            '''
                    }
                }
            }
        }
        
        stage('Indentifying Misconfigs Using Datree In Helm Charts'){
            steps{
                script{
                    withEnv(['DATREE_TOKEN=00f8c907-1338-42e6-b3ac-b0fbad5aeef8']) {
                        sh 'cd kubernetes/'
                        sh 'helm datree test myapp/'
                     }  
                }
            }
        }
       
   }
}

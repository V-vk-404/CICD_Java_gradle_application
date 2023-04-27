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
                                docker build -t 34.125.183.199:8083/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 34.125.183.199:8083 
                                docker push  34.125.183.199:8083/springapp:${VERSION}
                                docker rmi 34.125.183.199:8083/springapp:${VERSION}
                            '''
                    }
                }
            }
        }
        
        stage('Indentifying Misconfigs Using Datree In Helm Charts'){
            steps{
                script{
                    dir('kubernetes/') {
                          withEnv(['DATREE_TOKEN=625f617a-965b-4c77-a1d3-6eca30898eae'])  {
                                sh 'helm datree test myapp/'
                          }
                    }
                }
            }
        }
        
        stage("Pushing The Helm Charts To Nexus"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                          dir('kubernetes/') {
                             sh '''
                                 helmversion=$( helm show chart myapp | grep version | cut -d: -f 2 | tr -d ' ')
                                 tar -czvf  myapp-${helmversion}.tgz myapp/
                                 curl -u admin:$docker_password http://34.125.183.199:8081/repository/helm-hosted/ --upload-file myapp-${helmversion}.tgz -v
                            '''
                          }
                    }
                }
            }
        }
   }
}

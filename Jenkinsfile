pipeline{
    agent any
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
        
        
       
   }
}

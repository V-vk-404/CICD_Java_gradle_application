pipeline{
    agent any
    stages{
        stage('SonarScanner analysis'){
           
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube'
                    }
                     waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'

                   }  
               }
            
        }
       
}
}

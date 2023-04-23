pipeline{
    agent any
    stages{
        stage('SonarScanner analysis'){
           
            steps{
                script{
                    withSonarQubeEnv(installationName: 'sq1') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube'
                    }
                     waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'

                   }  
               }
            
        }
       
}
}

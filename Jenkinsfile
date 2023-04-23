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

                   }  
               }
        }
        stage('SonarScanner Quality Gate '){
           
            steps{
                script{
                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                      }

                   }  
               }
        }
}
}

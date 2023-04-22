pipeline{
    agent any
    stages{
        stage("sonar quality check"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(installationName: 'sq1') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube'
                    }

                   }  
               }
        }
}
}

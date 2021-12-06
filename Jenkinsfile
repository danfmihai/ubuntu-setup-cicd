pipeline{
    agent any
    stages{
        stage("Sonar quality check"){
            agent {
                docker{
                    image:open-jdk:11
                }
            }            
            step{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                }
            }
        }
    }
}
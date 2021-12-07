pipeline {
    agent any

    environment{
        VERSION="${env.BUILD_ID}"
    }

    stages {
        stage('sonar quality check') {
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'chmod +x gradlew'
                            sh 'java -version'
                            sh './gradlew sonarqube'
                    }
                timeout(1) {
                    def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                }    
                }  
            }
        }
        stage("docker build & docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                        sh '''
                        docker build -t 192.168.102.171:8083/springapp:${VERSION} .
                        docker login -u admin -p $docker_password 192.168.102.171:8083
                        docker push 192.168.102.171:8083/springapp:${VERSION}
                        docker rmi 192.168.102.171:8083/springapp:${VERSION}
                        '''
                    }
                }
            }
        }
        post {
            always {
                mail bcc: '', body: '''<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: {env.BUILD_URL}''', cc: '', from: '', replyTo: '', subject: '${currentBuild.result} CI: Project name -> ${env.JOB_NAME}', to: 'misu3108@yahoo.com'
            }
        }
    }      
}
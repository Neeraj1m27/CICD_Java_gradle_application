pipeline{
    agent any
    stages{
        stage("sonar quality check"){
            agent{
                docker {
                    image "openjdk:11"
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'jenkinsrepo') {
                         sh 'chmod -R 777 gradlew'
                     sh './gradlew sonarqube'
     
                          }
            

                }
                
            }
            
        }
    }
    
}
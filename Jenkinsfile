pipeline{
    agent any
    stages{
        stage("sonar quality check"){
        //  agent{
          //      docker {
           //         image 'openjdk:11'
           //   }
           // }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'jenkinsrepo') {
                         sh '''
                       chmod -R 777 gradlew
                      ./gradlew sonarqube
                       
                       '''
     
                          }
                  //         timeout(time: 1, unit: 'HOURS') {
                   //   def qg = waitForQualityGate()
                    //  if (qg.status != 'OK') {
                    //       error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    //  }
                    }
            

                }
                
            }
            
        }
    }
    
 }
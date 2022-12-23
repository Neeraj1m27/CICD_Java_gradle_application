pipeline{
    agent any
 //   environment{
  //    VERSION = "${env=BUILD_ID}"
   // }
    stages{
       stage("sonar qube analysis") {
           //  agent{ 
              
            //   docker { image 'openjdk:11' }
          //  }
            steps{
                script{
                  withSonarQubeEnv(credentialsId: 'sonarqube1') {
    
                     sh '''
                       chmod -R 777 gradlew
                      ./gradlew sonarqube 
                     
                       '''
                   }
                   timeout(5) {
                     def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
                     }
            }
       }
    }

}
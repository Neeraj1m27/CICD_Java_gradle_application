pipeline{
    agent any
    environment {
      VERSION = "${env.BUILD_ID}"
    }
    stages{
       stage("sonar qube analysis") {
            //  agent{ 
              
            //   docker { image 'openjdk:11' }
          //  }
            steps{
                script{
                  withSonarQubeEnv(credentialsId: 'dockersonar') {
    
                     sh '''
                       chmod -R 777 gradlew
                      ./gradlew sonarqube 
                     
                       '''
                   }
   //        timeout(5) {
     //                def qg = waitForQualityGate()
       //               if (qg.status != 'OK') {
         //                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
           //           }
    //                }
                }
            }
       }
                  
       
                

       
    

     
            
    stage("build docker image"){
     steps{
        script{
          withCredentials([string(credentialsId: 'admin1', variable: 'password')]) {

             sh '''
            docker build -t 192.168.2.168:8083/springapp:${VERSION} .
            docker login -u admin -p $password 192.168.2.168:8083
            docker push 192.168.2.168:8083/springapp:${VERSION}
           docker rmi 192.168.2.168:8083/springapp:${VERSION}
            docker image prune -f
          '''
                
}
       }
    }
   }

    }

}
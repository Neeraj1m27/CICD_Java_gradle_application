pipeline{
    agent any
     environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage("sonar quality check"){
         //agent{
           //    docker {
             //       image 'openjdk:11'
             // }
           // }
            steps{
                script{
                   withSonarQubeEnv(credentialsId: 'sona') {
    
                       sh '''
                       chmod -R 777 gradlew
                       ./gradlew sonarqube
                     
                       '''
                   }
                      timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
            

                
                
            }
        
            
        }



    }

stage("building docker image and pushing it to nexus"){
           steps{
               script{

               withCredentials([string(credentialsId: 'admin', variable: 'docker_pass')]) {
                    sh '''

                   docker build -t 192.168.2.168:8083/springapp:${VERSION} .
                   docker login -u admin -p $docker_pass 192.168.2.168:8083
                  docker push  192.168.2.168:8083/springapp:${VERSION}
                  docker rmi 192.168.2.168:8083/springapp:${VERSION}  
                  docker image prune -f      
                  '''
                  }
                }
              }
           }


}
}
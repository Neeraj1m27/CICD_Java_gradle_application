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
     //     timeout(5) {
                 //   def qg = waitForQualityGate()
                 //    if (qg.status != 'OK') {
                  //        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                 // }
                 // }
                }
            }
       }

    stage("build docker image"){
     steps{
        script{
      
         

             sh '''
            docker build -t neeraj1m19/devopsone:${VERSION} .
            echo neeraj@25 | docker login -u neeraj1m19 --password-stdin
            docker push neeraj1m19/devopsone:${VERSION}
           docker rmi neeraj1m19/devopsone:${VERSION}
            docker image prune -f
          '''
                
                    
               }
           }
        }
           
         stage("update build tag in helm"){
     steps{
        script{
          dir('kubernetes') {
            withCredentials([gitUsernamePassword(credentialsId: 'scp', gitToolName: 'Default')]) {
             sh '''

           scp -r springboot/ getkart@192.168.2.28:/home/getkart/ 

             helm upgrade --install springboot getkart@192.168.2.28:/home/getkart/springboot/ --set image.repository="neeraj1m19/devopsone" --set image.tag=${VERSION}

          '''
            }
          }   
                    
               }
           }
        }
    

           }

    post {
		always {
			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "neerajtank94@gmail.com";  
		}
	}
}
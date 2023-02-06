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
      
          withCredentials([usernameColonPassword(credentialsId: 'dockerhublogin', variable: 'dockerhublogin')]) {

             sh '''
            docker build -t neeraj1m19/devopsone:${VERSION} .
            docker login -u Neeraj1m19 -p $dockerhublogin https://index.docker.io/v1/
            docker push neeraj1m19/devopsone:${VERSION}
           docker rmi neeraj1m19/devopsone:${VERSION}
            docker image prune -f
          '''
                
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
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

      stage("this is datree"){
              agent{ 
              
               docker { image 'fluxcd/helm-operator:1.4.4' }
           }

        steps{ 
          script{
               dir('kubernetes') {
                  withEnv(['DATREE_TOKEN=05e0ff60-fd92-4e55-a87c-78be62f889aa']) {
    
           
                    sh 'helm datree test myapp/'
           
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
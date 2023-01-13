pipeline{
    agent any
    stages{
       stage("datree test") {
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

   
}
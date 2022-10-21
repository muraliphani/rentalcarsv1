node(){

   stage("Git Checkout"){
   checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'repo3git', url: 'https://github.com/muraliphani/rentalcarsv1.git']]])
   }
   
   stage("Maven Build"){
   sh "mvn package"
   } 
}

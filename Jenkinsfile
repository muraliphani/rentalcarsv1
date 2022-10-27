node(){

   stage("Git Checkout"){
   checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'repo3git', url: 'https://github.com/muraliphani/rentalcarsv1.git']]])
   }
   
   stage("Maven Build"){
   sh "mvn package"
   } 
   
   stage("sonarqube"){
       scannerHome = tool 'SonarQubeScanner'
       withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
  
 }
   stage("Upload to nexus"){
   nexusArtifactUploader artifacts: [[artifactId: '$BUILD_ID', classifier: '', file: 'target/RentalCars.war', type: 'war']], 
   credentialsId: 'nexusrepologin', groupId: 'prod', nexusUrl: '35.174.204.118:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'devtest1', version: '$BUILD_ID'
  
  }
   
    stage("Deploy to tomcat"){
  sshagent(['tomcatpem']) {
    sh "scp -o StrictHostKeyChecking=no target/RentalCars.war ec2-user@34.235.88.114:/opt/apache-tomcat-10.0.27/webapps"
}
}

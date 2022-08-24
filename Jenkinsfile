node(){

   stage("Git Checkout"){
   checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/muraliphani/rentalcarsv1.git']]])

   }

   stage("Maven Build"){
   sh "mvn package"
   }

   stage("Sonar Analysis"){
       scannerHome = tool 'sonarqubescanner'
       withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
       timeout(time: 1, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }

   }

   stage("upload to nexus"){
    nexusArtifactUploader artifacts: [[artifactId: '$BUILD_ID', classifier: '', file: 'target/rentalcarsv1.war', type: 'war']],
    credentialsId: 'nexus', groupId: 'prod', nexusUrl: '34.216.171.130:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'rentalcarsv1', version: '$BUILD_ID'

   }
}

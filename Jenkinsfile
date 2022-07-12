node(){

   stage("Git Checkout"){
   checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/muraliphani/RentalCars.git']]])

   }

   stage("Maven Build"){
   sh "mvn package"
   }

   stage("Sonar Analysis"){
       scannerHome = tool 'SonarQubeScanner'
       withSonarQubeEnv('sonar') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
       timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }

   }

   stage("upload to nexus"){
    nexusArtifactUploader artifacts: [[artifactId: '$BUILD_ID', classifier: '', file: 'target/RentalCars.war', type: 'war']],
    credentialsId: 'nexus', groupId: 'prod', nexusUrl: '54.177.152.94:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'RentalCars', version: '$BUILD_ID'

   }
}
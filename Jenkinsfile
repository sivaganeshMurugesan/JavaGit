node{
  stage('Clone') {
        checkout scm
    }
  stage('Package and sonarqube analysis '){
      
    withSonarQubeEnv('sonarqubeScanner') {
       sh "mvn clean package -Dv=${BUILD_NUMBER} sonar:sonar"
    }
  }
  stage('nexus upload'){
      nexusArtifactUploader artifacts: [[artifactId: 'myweb', 
                                         classifier: '', 
                                         file: "target/myweb-${BUILD_NUMBER}.war", 
                                         type: 'war']],
      credentialsId: 'NexusIntegration', 
      groupId: 'in.javahome', 
      nexusUrl: 'http://13.68.212.139:8083', 
      nexusVersion: 'nexus3',
      protocol: 'http',
      repository: 'Siva_poc', 
      version: '1.0.0'
  }
  
}

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
    nexusArtifactUploader artifacts: [[artifactId: 'myweb', classifier: '', file: 'target/myweb-${BUILD_NUMBER}.war', type: 'war']], credentialsId: 'd922f515-57fb-480d-af12-ab94728b76b2', groupId: 'in.javahome', nexusUrl: '10.0.0.4:8083', nexusVersion: 'nexus3', protocol: 'http', repository: 'poc1', version: '1.0.0'
  }
  stage ('Build Docker Image'){
    sh "docker build -t sivaganesh1625977/myapp:${BUILD_NUMBER} ."
  }
  
}

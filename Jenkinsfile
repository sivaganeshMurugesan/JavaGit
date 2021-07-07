node{
  stage('Clone') {
        checkout scm
    }
  stage('package'){
    def mvnHome= tool name: 'Maven', type: 'maven'
    sh "${mvnHome}/bin/mvn package"
  }
  stage ('sonarqube analysis'){
    def mvnHome= tool name: 'Maven', type: 'maven'
    withSonarQubeEnv('sonarqubeScanner') {
       sh "${mvnHome}/bin/mvn sonar:sonar"
    }
  }
  stage('nexus upload'){
      nexusArtifactUploader artifacts: [[artifactId: 'myweb', 
                                         classifier: '', 
                                         file: 'target/myweb-1.0.0.war', 
                                         type: 'war']],
      credentialsId: 'NexusIntegration', 
      groupId: 'in.javahome', 
      nexusUrl: '10.0.0.13:8081', 
      nexusVersion: 'nexus3',
      protocol: 'http',
      repository: 'POC1', 
      version: '1.0.0'
  }
  stage ('Build Docker image'){
    sh 'docker build -t sivaganesh/myapp:1.0.0 .'
  }
  stage ('Push Docker image'){
    withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerHubPwd')]) {
    // some block
      sh "docker login -u sivaganesh1625977 -p ${dockerHubPwd}"
    }
    docker tag 'sivaganesh/myapp:1.0.0' 'sivaganesh1625977/sivaganesh/myapp:1.0.0'
    sh 'docker push sivaganesh1625977/sivaganesh/myapp:1.0.0'

  }
}

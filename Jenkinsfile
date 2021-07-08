node{
  stage('Clone') {
        checkout scm
    }
  stage('package'){
    def mvnHome= tool name: 'Maven', type: 'maven'
    sh "${mvnHome}/bin/mvn package -Dv=${BUILD_NUMBER}"
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
  stage ('Build Docker Image'){
    sh 'docker build -t sivaganesh1625977/myapp:72.0.0 .'
  }
  stage ('Push Docker Image'){
    withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerHubPwd')]) {
    // some block
      sh "docker login -u sivaganesh1625977 -p ${dockerHubPwd}"
    }
    sh 'docker push sivaganesh1625977/myapp:72.0.0'
  }
    stage ('K8S Deploy') {
      withCredentials([file(credentialsId: 'K8s', variable: 'KUBECRED')]) {
            
            sh 'cat $KUBECRED > ~/.kube/config'
            sh 'kubectl apply -f deployment.yaml'
      }          
        }
}

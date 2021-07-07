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

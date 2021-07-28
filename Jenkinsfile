node{
  stage('Clone') {
        checkout scm
    }
  stage('Package'){
      
    withSonarQubeEnv('sonarqubeScanner') {
       sh "mvn clean package -Dv=${BUILD_NUMBER} sonar:sonar"
    }
  }
  
}

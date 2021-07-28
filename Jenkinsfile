node{
  stage('Clone') {
        checkout scm
    }
  stage('Package'){
    //def mvnHome=tool name: 'maven', type: 'maven'
    sh "${mvnHome}/bin/mvn clean package"    
  }
}

node{
  stage('Clone') {
        checkout scm
    }
  stage('Package'){
    //def mvnHome=tool name: 'maven', type: 'maven'
    sh "mvn clean package"    
  }
}

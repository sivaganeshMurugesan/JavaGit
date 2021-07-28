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
    nexusArtifactUploader artifacts: [[artifactId: 'myweb', classifier: '', file: 'target/myweb-${BUILD_NUMBER}.war', type: 'war']], credentialsId: 'd922f515-57fb-480d-af12-ab94728b76b2', groupId: 'in.javahome', nexusUrl: '10.0.0.4:8083', nexusVersion: 'nexus3', protocol: 'http', repository: 'poc1', version: "${BUILD_NUMBER}"
  }
  stage ('Build Docker Image'){
    sh "docker build -t sivaganesh1625977/myapp:${BUILD_NUMBER} ."
  }
  stage ('Push Docker Image'){
    withCredentials([string(credentialsId: 'dockerpwds', variable: 'dockerpwd')]) {
      sh "docker login -u sivaganesh1625977 -p ${dockerpwd}"
    }
    sh "docker push sivaganesh1625977/myapp:${BUILD_NUMBER}"
  }
  stage ('K8S Deploy') {
      withCredentials([file(credentialsId: 'kubeconfig', variable: 'FILE')]) {
        sh """sed -i "s@imagesdep@sivaganesh1625977/myapp:${BUILD_NUMBER}@g" /var/lib/jenkins/workspace/Siva_pipeline/deployment.yaml"""
            sh 'kubectl --kubeconfig=$FILE apply -f deployment.yaml'
      }          
   }
  
}

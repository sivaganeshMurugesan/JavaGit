node{
  stage('SCM clone'){
    git 'https://github.com/sivaganeshMurugesan/JavaGit'
  }
  stage('package'){
    def mvnHome= tool name: 'Maven', type: 'maven'
    sh '${mvnHome}/bin/mvn package'

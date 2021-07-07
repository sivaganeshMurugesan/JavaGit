node{
  stage('SCM clone'){
    git 'https://github.com/sivaganeshMurugesan/JavaGit'
  }
  stage('package'){
    def mvn-home= tool name: 'Maven', type: 'maven'
    sh '${mvn-home}/bin/mvn package'

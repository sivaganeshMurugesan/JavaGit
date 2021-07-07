node{
  stage('SCM clone'){
    git 'https://github.com/sivaganeshMurugesan/JavaGit'
  }
  stage('package'){
    def mvnhome= tool name: 'Maven', type: 'maven'
    sh '${mvnhome}/bin/mvn package'

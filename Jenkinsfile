pipeline {
  // agent { label 'linux' }
  agent any
tools {
  maven 'maven3'
}
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    CI = true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
  }
  stages {
    stage('Build') {
      steps {
        sh script: 'mvn clean install'
      }
    }
    stage('Upload to Artifactory') {
      agent {
        docker {
          image 'releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0' 
          reuseNode true
        }
      }
      steps {
        sh 'jfrog rt upload --url http://13.80.27.114:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/demo-0.0.1-SNAPSHOT.jar java-web-app/'
      }
    }
  }
}
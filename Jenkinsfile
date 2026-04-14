pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'Compiling sysfoo app...'
        sh 'mvn compile'
      }
    }

    stage('test') {
      steps {
        echo 'Testing sysfoo app...'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      steps {
        echo 'Packaging sysfoo app...'
        sh '''GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)

mvn versions:set -DnewVersion="$GIT_SHORT_COMMIT"
mvn versions:commit'''
        sh 'mvn package'
        archiveArtifacts '**/target/*.jar'
      }
    }

  }
  tools {
    maven 'maven_3.9.14'
  }
  post {
    always {
      echo 'This pipeline is completed.'
    }

  }
}
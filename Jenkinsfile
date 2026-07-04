pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'Compiling sysfoo app...'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17-alpine'
        }

      }
      steps {
        echo 'Testing sysfoo app...'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      parallel {
        stage('package') {
          agent {
            docker {
              image 'maven:3.9.6-eclipse-temurin-17-alpine'
            }

          }
          steps {
            echo 'Packaging sysfoo app...'
            sh '''GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)

mvn versions:set -DnewVersion="$GIT_SHORT_COMMIT"
mvn versions:commit'''
            sh 'mvn package'
            archiveArtifacts '**/target/*.jar'
          }
        }

        stage('dockerBP') {
          steps {
            script {
              docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin')
              {
                def commitHash = env.GIT_COMMIT.take(7)
                def dockerImage = docker.build("ervinblack/sysfoo:${commitHash}", "./")
                dockerImage.push()
                dockerImage.push("latest")
                dockerImage.push("dev")
              }
            }

          }
        }

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
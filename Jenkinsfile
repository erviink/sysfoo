pipeline {
    agent any
    tools {
       maven 'maven_3.9.14' 
    }
    stages {
        stage("build") {
            steps {
                echo 'Compiling sysfoo app...'
                sh 'mvn compile'
            }
        }
        stage("test") {
            steps {
                echo 'Testing sysfoo app...'
                sh 'mvn clean test'
            }
        }
        stage("package") {
            steps {
                echo 'Packaging sysfoo app...'
                sh 'mvn package'
            }
        }
    }
    post {
        always {
            echo 'This pipeline is completed.'
        }
    }
}

pipeline {
  agent any

  tools {
    maven "apache-maven-3.6.3"
  }

  stages {
    stage('Build') {
      steps {
        // original - git 'https://github.com/ajlanghorn/dvja.git'
        git 'https://github.com/AntonioFeijaoUK/aws-ako2020-dvja.git'
        sh "mvn clean package"
      }
    }
    stage('Publish to S3') {
      steps {
        sh "aws s3 cp /var/lib/jenkins/workspace/dvja/target/dvja-1.0-SNAPSHOT.war s3://aws-ako20-cicd-security-buildartifacts-1v2n6il6nr4m4/dvja-1.0-SNAPSHOT.war"
      }
    }
    stage('Tidy up') {
      steps {
        cleanWs()
      }
    }
  }
}

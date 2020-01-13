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
    stage('Scan for vulnerabilities') {
      steps {
        sh 'java -jar dvja-*.war && zap-cli quick-scan --self-contained --spider -r http://127.0.0.1 && zap-cli report -o zap-report.html -f html'
      }
    }
    stage('Check dependencies') {
      steps {
        dependencyCheck additionalArguments: '', odcInstallation: 'Dependency-Check'
        //dependencyCheckPublisher failedTotalCritical: 1, failedTotalHigh: 1, failedTotalLow: 10, failedTotalMedium: 5, pattern: '', unstableTotalCritical: 1, unstableTotalHigh: 1, unstableTotalLow: 10, unstableTotalMedium: 5
        dependencyCheckPublisher pattern: ''
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

  post {
    always {
        archiveArtifacts artifacts: 'zap-report.html', fingerprint: true
  }
  
}

}
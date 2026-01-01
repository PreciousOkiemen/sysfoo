pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building...'
        sh 'mvn compile'
      }
    }

    stage('Test') {
      steps {
        echo 'Testing...'
        sh 'mvn clean test jacoco:report'
      }
    }

    stage('Package') {
      environment {
        SONAR_TOKEN = credentials('SONARQUBE_TOKEN')
      }
      steps {
        echo 'Packaging...'
        sh '''# Truncate the GIT_COMMIT to the first 7 characters
GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)

# Set the version using Maven
mvn versions:set -DnewVersion="$GIT_SHORT_COMMIT"
mvn versions:commit'''
        sh 'mvn package -DskipTests sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${SONARQUBE_TOKEN} -Dsonar.projectKey=sysfoo'
        archiveArtifacts(artifacts: '**/target/*.jar', fingerprint: true)
        archiveArtifacts '**/target/*.jar'
      }
    }

  }
  tools {
    maven 'Maven3.9.11'
  }
  post {
    always {
      echo 'Cleaning up or finishing the build...'
    }

    success {
      echo 'Pipeline completed successfully!'
    }

    failure {
      echo 'Pipeline failed. Check build and test logs.'
    }

  }
  triggers {
    pollSCM('H/2 * * * *')
  }
}
pipeline {
    agent any
  
    triggers {
        pollSCM('H/2 * * * *')
    }

    
    tools {
        maven 'Maven3.9.11'
    }

    
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
            steps {
                echo 'Packaging...'
                sh 'mvn package -DskipTests sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=squ_e1f51073124232934de7989fa70bc765afddea9f -Dsonar.projectKey=sysfoo'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
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
}

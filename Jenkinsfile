pipeline {
    agent any
  
    triggers {
        pollSCM('H/2 * * * *')
    }

    
    tools {
        maven 'Maven 3.6.1'
    }

    
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn -f worker/pom.xml compile'
            }
        }


        stage('Test') {
            steps {
                echo 'Testing...'
                sh 'mvn -f worker/pom.xml test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging...'
                sh 'mvn -f worker/pom.xml package -DSkipTests'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
    
  
    post {
        always {
            
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check build and test logs.'
        }
    }
}

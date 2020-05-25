pipeline {
    agent any
    tools {
        maven 'Default'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to staging') {
            steps {
                build job: 'maven-project-deploy-to-staging'
            }
        }
    }
}
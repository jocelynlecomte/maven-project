pipeline {
    agent any
    parameters {
        string(name: 'staging', defaultValue: '15.188.80.89', description: 'IP of the EC2 staging instance')
        string(name: 'prod', defaultValue: '35.180.211.126', description: 'IP of the EC2 production instance')
    }
    tools {
        maven 'Default'
    }
    triggers {
        pollSCM('H * * * *')
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
        stage('Deployments') {
            parallel {
                stage('Deploy to staging') {
                    steps {
                        sh "scp -i /var/jenkins_home/jenkins.pem **/target/*.war ec2-user@${params.staging}:/var/lib/tomcat/webapps"
                    }
                }
                stage('Deploy to production') {
                    steps {
                        sh "scp -i /var/jenkins_home/jenkins.pem **/target/*.war ec2-user@${params.prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}
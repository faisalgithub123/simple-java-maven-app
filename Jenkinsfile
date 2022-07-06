pipeline {
    agent {
        label 'maven'
    }
    environment {
        DEPLOY_SERVER = "172.31.30.68"
        DEPLOY_USER = "ec2-user"
        DEPLOY_PKG = "my-app-1.0-SNAPSHOT.jar"
    }
    tools {
        maven 'maven-latest'
    }
    stages {
        stage('Build & Report') {
            steps {
                sh "mvn clean package surefire-report:report-only"
            }
            post {
                success {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                    publishHTML([allowMissing: true, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site', reportFiles: 'surefire-report.html', reportName: 'HTML Report', reportTitles: ''])
                }
            }
        }
        stage('Upload') {
            steps {
                echo "jfrog commands go here"
                script {
                    myFunction("Sample Test")
                }
            }
        }
        stage('Sonar Analysis') {
            steps {
                sh "echo 'mvn sonar:sonar'"
            }
        }
        stage('Static Analysis') {
            steps {
                echo "Hp Fortity, AquaSec, Coverity"
            }
        }
        stage('Deploy to Dev') {
            steps {
                sshagent(['project-a-dev-ssh-key']) {
                    sh "scp -o StrictHostKeyChecking=no target/${DEPLOY_PKG} ${DEPLOY_USER}@${DEPLOY_SERVER}:/tmp"
                }
            }
        }
    }
}

def myFunction(myvar) {
    echo "This is called by a function"
}
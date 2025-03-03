pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1') 
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/awaypotato123/spring-petclinic.git'
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    sh './mvnw clean verify'
                }
            }
        }

        stage('Code Coverage') {
            steps {
                script {
                    sh './mvnw jacoco:report'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Publish Jacoco Report') {
            steps {
                jacoco execPattern: '**/target/jacoco.exec'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}

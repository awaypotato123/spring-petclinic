pipeline {
    agent any

    tools {
        maven 'M3'  
    }

    triggers {
        cron('H/3 * * * 1')  
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/awaypotato123/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }

        stage('Code Coverage') {
            steps {
                sh "mvn jacoco:prepare-agent test jacoco:report"
            }

            post {
                success {
                    jacoco execPattern: '**/target/jacoco.exec'
                }
            }
        }
    }
}


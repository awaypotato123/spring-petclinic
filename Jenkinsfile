pipeline {
    agent any

    tools {
        maven 'MAVEN3'  
    }

    triggers {
        cron('H/3 * * * 1')  // Every 3 minutes on Mondays
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
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
                    jacoco(
                        execPattern: '**/target/jacoco.exec',
                        classPattern: '**/target/classes',
                        sourcePattern: '**/src/main/java'
                    )
                }
            }
        }
    }
}
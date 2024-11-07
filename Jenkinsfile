pipeline {
    agent any

    triggers {
        cron('H/10 * * * 1')  // Triggers every 10 minutes on Mondays
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'  // Build the project using Maven
                }
            }
        }

        stage('Code Coverage') {
            steps {
                script {
                    sh 'mvn test jacoco:report'  // Generate JaCoCo code coverage
                }
            }

            post {
                always {
                    jacoco execPattern: '**/target/jacoco.exec', 
                           classPattern: '**/target/classes', 
                           sourcePattern: '**/src/main/java', 
                           exclusionPattern: '**/target/generated-sources/**'
                }
            }
        }

        stage('Generate Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: false
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean workspace after build
        }
    }
}

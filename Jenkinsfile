groovy
pipeline {
    agent { label 'Jenkins-Agent' }

    tools {
        jdk 'Java21'
        maven 'Maven3'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main',
                    credentialsId: 'github',
                    url: 'https://github.com/TanweerAlam2/register-app'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn -B clean package'
            }
        }

        stage('Test Application') {
            steps {
                sh 'mvn -B test'
            }
        }
    }

    post {
        always {
            junit testResults: '**/target/surefire-reports/*.xml',
                  allowEmptyResults: true

            archiveArtifacts artifacts: '**/target/*.jar, **/target/*.war',
                             allowEmptyArchive: true
        }

        success {
            echo 'Build and test pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the stage logs for the error.'
        }
    }
}

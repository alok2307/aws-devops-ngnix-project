
pipeline {
    agent any

    options {
        timeout(time: 5, unit: 'MINUTES')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub'
            }
        }

        stage('Build') {
            steps {
                script {
                    try {
                        echo 'Starting build validation'
                        sh 'test -f missing.html'
                        echo 'Build validation successful'
                    } catch (Exception e) {
                        echo "Build error detected: ${e.getMessage()}"
                        throw e
                    } finally {
                        echo 'Build stage execution completed'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying website to Nginx'
                sh 'sudo /opt/jenkins-deploy/deploy.sh'
            }
        }

        stage('Verify') {
            steps {
                sh 'curl -f -I http://localhost'
                echo 'Deployment verification successful'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed'
        }
        success {
            echo 'SUCCESS: Website deployment completed!'
        }
        failure {
            echo 'FAILURE: Please check Jenkins Console Output!'
        }
    }
}


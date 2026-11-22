
pipeline {
    agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'prod'],
            description: 'Select deployment environment'
        )
    }

    environment {
        APP_NAME = 'aws-nginx-project'
        DEPLOY_ENV = "${params.ENVIRONMENT}"
    }

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
                        echo "Application: ${env.APP_NAME}"
                        echo "Selected Environment: ${env.DEPLOY_ENV}"

                        withCredentials([
                            string(
                                credentialsId: 'alok2307',
                                variable: 'GITHUB_TOKEN'
                            )
                        ]) {
                            sh '''
                                if [ -n "$GITHUB_TOKEN" ]; then
                                    echo "GitHub credential loaded successfully"
                                else
                                    echo "Credential not found"
                                    exit 1
                                fi
                            '''
                        }

                        echo 'Starting build validation'

                        sh 'test -f index.html'

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
                echo "Deploying to ${env.DEPLOY_ENV}"

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

            emailext(
                subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",

                body: """
Hello,

Jenkins Pipeline completed successfully.

Application: ${env.APP_NAME}
Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Environment: ${env.DEPLOY_ENV}

Deployment Status: SUCCESS
Website Status: HTTP 200 OK

Build URL: ${env.BUILD_URL}

Regards,
Jenkins DevOps Pipeline
""",

                to: "kumaralokbkgmail.com"
            )
        }

        failure {
            echo 'FAILURE: Please check Jenkins Console Output!'

            emailext(
                subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",

                body: """
Hello,

Jenkins Pipeline execution failed.

Application: ${env.APP_NAME}
Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Environment: ${env.DEPLOY_ENV}

Deployment Status: FAILURE

Please check Jenkins Console Output.

Build URL: ${env.BUILD_URL}

Regards,
Jenkins DevOps Pipeline
""",

                to: "kumaralokbk@gmail.com"
            )
        }
    }
}


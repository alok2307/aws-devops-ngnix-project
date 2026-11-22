pipeline {
    agent any

    options {
        timeout(time: 5, unit: 'MINUTES')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is checked out from GitHub'
            }
        }

        stage('Build') {
            steps {
                echo 'Building website project'
                sh 'ls -l'
                sh 'test -f index.html'
                echo 'Build validation successful'
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
}

pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Clean Up Code') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies & Build') {
            steps {
                sh '''
                    ls -l
                    node --version
                    npm --version
                    npm install
                    npm run build
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    npm run test
                '''
            }
        }
    }
}

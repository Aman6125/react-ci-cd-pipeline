pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
        timestamps()
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test inside Docker') {
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args "-u root -v /tmp/npm-cache:/root/.npm -v ${WORKSPACE}:/app"
                    reuseNode true
                }
            }
            steps {
                sh '''
                    cd /app

                    echo "Node Version:"
                    node --version
                    echo "NPM Version:"
                    npm --version

                    echo "Install Dependencies"
                    npm install --legacy-peer-deps --no-audit --no-fund

                    echo "Build"
                    npm run build

                    echo "Test"
                    npm run test
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Build & Test completed successfully"
        }
        failure {
            echo "❌ Build failed — check the logs above"
        }
    }
}

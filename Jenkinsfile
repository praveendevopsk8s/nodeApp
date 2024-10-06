pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-u root' // Optional: Run as root to avoid permission issues
                    reuseNode true
                }
            }
            steps {
                script {
                    cleanWs() // Clean the workspace before building
                }
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    args '-u root' // Optional: Run as root to avoid permission issues
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html || { echo "Build failed, index.html not found!"; exit 1; }
                    npm test
                '''
            }
        }        
    }

    post {
        always {
            // Archive test results (ensure your tests generate results in this format)
            junit 'test-results/junit.xml'
            cleanWs() // Clean up the workspace after the build
        }
    }
}

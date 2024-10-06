pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                withEnv(['NODE_OPTIONS=--openssl-legacy-provider']) {
                    sh '''
                        ls -la
                        node --version
                        npm --version
                        cleanWs()
                        npm ci
                        npm run build
                        ls -la
                    '''
                }
            }
        }
        stage('Test Praveen1') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }	
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    # Create a directory for global npm installs in the user's home directory
                    mkdir -p ~/.npm-global
                    # Set npm to use this directory for global installs
                    npm config set prefix '~/.npm-global'
                    # Update the PATH to include the new directory
                    export PATH=$HOME/.npm-global/bin:$PATH
                    # Install serve
                    npm install serve
                    # Serve the build and run tests
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }		
    }
    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }	
}

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
                    // reuseNode true
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
                    # Use a writable directory for npm global installs
                    export HOME=/tmp/jenkins
                    mkdir -p $HOME/.npm-global
                    
                    # Set npm to use this directory for global installs
                    npm config set prefix="$HOME/.npm-global"
                    
                    # Update the PATH to include the new directory
                    export PATH=$HOME/.npm-global/bin:$PATH
                    
                    # Debugging: Print HOME and current user
                    echo "HOME: $HOME"
                    echo "Current User: $(whoami)"
                    
                    # Install serve globally
                    npm install -g serve
                    
                    # Serve the build and run tests
                    nohup serve -s build &
                    sleep 10
                    
                    # Run Playwright tests
                    npx playwright test --reporter=html
                '''
            }
        }		
    }
    post {
        always {
            junit 'jest-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }	
}

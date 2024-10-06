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
                // Set the NODE_OPTIONS environment variable
                withEnv(['NODE_OPTIONS=--openssl-legacy-provider']) {
                    sh '''
                        ls -la
                        node --version
                        npm --version
			cleanWs()
                        npm install
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
                    reuseNode  true
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
                    npm install serve
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

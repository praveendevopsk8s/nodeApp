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
   			rm -rf node_modules
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
    }
}

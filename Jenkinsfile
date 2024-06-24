pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Checking out the repository...'
                    checkout scm
                    echo 'Repository checked out successfully'
                }
            }
        }

        stage('Verify Package.json') {
            steps {
                script {
                    echo 'Verifying presence of package.json...'
                    sh 'ls -la'
                    sh 'if [ ! -f package.json ]; then echo "package.json not found"; exit 1; fi'
                    sh 'cat package.json'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the application...'
                    sh 'npm run build'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'npm test'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying the application...'
                    sh '''
                        scp -r ./build user@your-server:/path/to/deploy
                        ssh user@your-server 'pm2 restart your-app'
                    '''
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Cleaning up workspace...'
                deleteDir()
            }
        }
        success {
            script {
                echo 'Build and deployment successful!'
            }
        }
        failure {
            script {
                echo 'Build or deployment failed!'
            }
        }
    }
}

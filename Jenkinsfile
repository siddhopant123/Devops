pipeline {
    agent any

    environment {
        // Define environment variables here if needed
        NODE_ENV = 'production'
    }

    triggers {
        githubPush()  // Trigger pipeline on GitHub push events
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the latest code from the GitHub repository
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Build the application
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the application
                // Example: You can use SSH to deploy to a remote server
                sh '''
                    scp -r ./build user@your-server:/path/to/deploy
                    ssh user@your-server 'pm2 restart your-app'
                '''
            }
        }
    }

    post {
        always {
            // Clean up actions to run regardless of build result
            echo 'Cleaning up workspace'
            deleteDir() // Clean up the workspace
        }
        success {
            // Actions to run on successful build
            echo 'Build and deployment successful!'
        }
        failure {
            // Actions to run on failed build
            echo 'Build or deployment failed!'
        }
    }
}

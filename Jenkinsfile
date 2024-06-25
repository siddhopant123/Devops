pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'github-credentials' // Replace with the ID of the credentials you set up in Jenkins
        REPO_URL = 'https://github.com/siddhopant123/Devops.git'
    }

    triggers {
        githubPush()  // Trigger the pipeline on GitHub push events
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the siddhu branch
                    checkout([$class: 'GitSCM', branches: [[name: '*/siddhu']], userRemoteConfigs: [[url: env.REPO_URL, credentialsId: env.GIT_CREDENTIALS_ID]]])
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

        stage('Merge to Main') {
            steps {
                script {
                    // Configure Git user
                    sh 'git config user.email "shelkesiddhopant@gmail.com"' // Replace with your Git email
                    sh 'git config user.name "Siddhopant123"' // Replace with your Git name

                    // Checkout the main branch
                    sh 'git checkout main'

                    // Merge siddhu branch into main branch
                    sh 'git merge origin/siddhu || true' // Use '|| true' to prevent the pipeline from failing

                    // Check for merge conflicts
                    def mergeConflict = sh(script: 'git diff --name-only --diff-filter=U', returnStdout: true).trim()

                    if (mergeConflict) {
                        echo "Merge conflicts detected in the following files:\n${mergeConflict}"

                        // Handle the merge conflicts here
                        // Example: abort the build and notify
                        error "Merge conflicts detected. Please resolve them manually."
                    } else {
                        // Push the changes to the main branch
                        withCredentials([usernamePassword(credentialsId: env.GIT_CREDENTIALS_ID, passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/siddhopant123/Devops.git main'
                        }
                    }
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

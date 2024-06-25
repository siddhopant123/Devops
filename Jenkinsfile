pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'github-credentials' // Use the ID of the credentials you set up in Jenkins
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

        stage('Merge to Main') {
            steps {
                script {
                    // Configure Git user
                    sh 'git config user.email "your-email@example.com"'
                    sh 'git config user.name "Your Name"'

                    // Checkout the main branch
                    sh 'git checkout main'

                    // Merge siddhu branch into main branch
                    sh 'git merge origin/siddhu'

                    // Push the changes to the main branch
                    withCredentials([usernamePassword(credentialsId: env.GIT_CREDENTIALS_ID, passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/siddhopant123/Devops.git main')
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Merge completed successfully!'
        }
        failure {
            echo 'Merge failed!'
        }
    }
}

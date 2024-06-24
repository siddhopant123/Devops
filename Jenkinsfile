pipeline {
    agent any

    triggers {
        githubPush()  // This trigger ensures the pipeline runs on every push to the repository
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // Add your build commands here
                // Example: sh 'make build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add your test commands here
                // Example: sh 'make test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add your deploy commands here
                // Example: sh 'make deploy'
            }
        }
    }
}

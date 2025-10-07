pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/rk179850/my-nodejs-app', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker-compose build web'
                }
            }
        }

        stage('Deploy Multi-Container App') {
            steps {
                script {
                    // Stop and remove containers if they exist
                    bat 'docker stop node-web node-mongo-db || exit 0'
                    bat 'docker rm node-web node-mongo-db || exit 0'

                    // Create .env file with BUILD_NUMBER for Docker Compose
                    writeFile file: '.env', text: "BUILD_NUMBER=${env.BUILD_NUMBER}"

                    // Deploy containers using the .env file
                    bat 'docker-compose --env-file .env up -d'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
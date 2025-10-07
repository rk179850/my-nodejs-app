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
                    bat 'docker stop node-web node-mongo-db || exit 0'
                    bat 'docker rm node-web node-mongo-db || exit 0'
                    bat "set BUILD_NUMBER=${env.BUILD_NUMBER} && docker-compose up -d"
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

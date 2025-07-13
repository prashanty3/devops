pipeline {
    agent any

    environment {
        COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/prashanty3/devops.git'
            }
        }

        stage('Start Services') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d --build'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'docker exec laravel-app composer install'
                sh 'docker exec laravel-app php artisan key:generate'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'docker exec laravel-app php artisan test'
            }
        }
    }

    post {
        always {
            sh 'docker-compose down'
        }
    }
}

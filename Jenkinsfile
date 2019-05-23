pipeline {
    agent { dockerfile true }

    stages {
        stage("Build and start test image") {
            steps {
                sh "cd files/"
                sh "docker-composer build"
                sh "docker-compose up -d"
            }
        }
        stage("Run tests") {
            steps {
                sh "docker-compose exec -T php-fpm composer --no-ansi --no-interaction tests-ci"
                sh "docker-compose exec -T php-fpm composer --no-ansi --no-interaction behat-ci"
            }
        }
    }
}

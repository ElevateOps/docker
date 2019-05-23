pipeline {
    agent { node { label 'librenms' } }

    stages {
        stage("Prepare") {
            steps {
        bitbucketStatusNotify buildState: "INPROGRESS"
        }
    }

        stage("Build and start test image") {
            steps {
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

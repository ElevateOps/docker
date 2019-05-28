pipeline {
    agent { label 'devel_at_insport' }

    stages {

        stage("Build and start image") {
            steps {
                sh "cd files/"
                sh "docker-compose up -d"
            }
        }

        stage("Tear down image - devel") {
            steps {
                sh "docker-compose down"
            }
        }
    }
}

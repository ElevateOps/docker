pipeline {
    agent { label 'devel_at_insport' }

    stages {

        stage("Build and start image") {
            steps {
                dir ("files") {
                    sh "ls -la"
                    sh "docker-compose up -d"
                }
            }
        }

        stage("Tear down image - devel") {
            steps {
                sh "docker-compose down"
            }
        }
    }
}

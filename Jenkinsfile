pipeline {
    agent { label 'devel_at_insport' }

    stages {

        stage("Tear down any previous containers") {
            steps {
                sh "cd /var/librenms && sudo docker-compose down"
            }
        }

        stage("Copy docker compose files") {
            steps {
                dir ("files") {
                    sh "sudo mkdir /var/librenms"
                    sh "sudo copy -r * /var/librenms"
                }
            }
        }

        stage("Spin-up the container") {
            steps {
                dir ("/var/librenms") {
                    sh "sudo docker-compose up -d"
                }
            }
        }

        stage("Tear down image - devel") {
            steps {
                dir ("/var/librenms") {
                    sh "sudo docker-compose down"
                }
            }
        }
    }
}

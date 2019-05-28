pipeline {
    agent { label 'devel_at_insport' }

    stages {

        stage("Tear down any previous containers") {
            try {
                sh "sudo mkdir -p /var/librenms"
                sh "cd /var/librenms && sudo docker-compose down"
            }
            catch (Exception e) {
                echo "Probably no docker container running. Check the logs…"
            }
        }

        stage("Copy docker compose files") {
            steps {
                dir ("files") {
                    sh "sudo cp -r * /var/librenms"
                }
            }
        }

        stage("Spin-up the container") {
            steps {
                sh "cd /var/librenms && sudo docker-compose up -d"
            }
        }
    }
}

pipeline {
    agent { label 'devel_at_insport' }

    stages {

        stage("Stop any previous containers") {
            steps {
                script {
                    try {
                        sh "sudo mkdir -p /var/librenms"
                        sh "cd /var/librenms && sudo docker-compose stop && sudo docker-compose rm"
                    }
                    catch (Exception e) {
                        echo "Probably no docker container running. Check the logs…"
                    }
                }
            }
        }

        stage("Copy docker compose files") {
            steps {
                dir ("files") {
                    sh "sudo cp -a . /var/librenms"
                }
            }
        }

        stage("Spin-up the container") {
            steps {
                sh "cd /var/librenms && sudo touch acme.json"
                sh "cd /var/librenms && sudo chmod 600 acme.json"
                sh "cd /var/librenms && sudo docker-compose up -d"
            }
        }

        stage("Configure Prometheus integration") {
            steps {
                dir ("files") {
                    sh """
                    cd /var/librenms && \
                    sudo cp prometheus-conf.txt /var/librenms/librenms/config/override.php
                    """
                }
            }
        }
    }
}

pipeline {
    agent { label 'devel_at_insport' }

    stages {

        stage("Tear down any previous containers") {
            steps {
                script {
                    try {
                        sh "sudo mkdir -p /var/librenms"
                        sh "cd /var/librenms && sudo docker-compose down"
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
                sh """
                cd /var/librenms && \
                sudo docker exec -i librenms sh -c 'echo "$config[\'prometheus\'][\'enable\'] = true;\n\$config['prometheus']['url'] = 'http://127.0.0.1:9091';\n\$config['prometheus']['job'] = 'librenms';" >> config.php'
                """
            }
        }
    }
}

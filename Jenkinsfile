pipeline {
    agent { label 'test_VM_proxmox' }

    stages {

        stage("Install docker & docker-compose") {
            steps {
                sh '''
                sudo apt-get -y remove docker docker-engine docker.io containerd runc
                sudo apt-get update -y
                sudo apt-get install -y\
                apt-transport-https \
                ca-certificates \
                curl \
                gnupg-agent \
                software-properties-common
                curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
                sudo add-apt-repository \
                "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
                $(lsb_release -cs) \
                stable"
                sudo apt-get update -y
                sudo apt-get install -y docker-ce docker-ce-cli containerd.io
                '''
                sh '''
                sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                sudo chmod +x /usr/local/bin/docker-compose
                '''
            }
        }

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
                    sh "ls -la"
                    sh "sudo cp -a . /var/librenms"
                }
            }
        }

        stage("Spin-up the container") {
            steps {
                sh "cd /var/librenms && ls -la"
                sh "cd /var/librenms && sudo touch acme.json"
                sh "cd /var/librenms && sudo chmod 600 acme.json"
                sh "sudo docker-compose --project-directory /var/librenms up -d"
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

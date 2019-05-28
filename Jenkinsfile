pipeline {
    agent {
        sshagent { label: devel_at_insport }
    }

    stages {

        stage("Build and start test image") {
            steps {
                sh "cd files/"
                sh "docker-compose up -d"
            }
        }

        stage("Run tests") {
            steps {
            }
        }
    }
}

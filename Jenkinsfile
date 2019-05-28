pipeline {
    agent {
        sshagent { credentials: ['elevate'] }
    }

    stages {

        stage("Establish connection") {
            steps {
                sh 'ssh -p 14339 elevate@193.106.100.89'
            }
        }

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

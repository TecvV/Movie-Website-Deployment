pipeline {
    agent any

    stages {
        stage('Checkout Code from GitLab') {
            steps {
                checkout([$class: 'GitSCM',
                  branches: [[name: 'main']],
                  userRemoteConfigs: [[
                      url: 'http://192.168.1.10/root/estate.git',
                      credentialsId: 'gitlab-credentials-id'
                  ]]
                ])
            }
        }

        stage('Deploy to Test Server') {
            steps {
                sshCommand remote: [
                    name: 'TestServer',
                    host: '192.168.1.20',
                    user: 'ubuntu',
                    identity: 'test-server-ssh'
                ], command: '''
                    sudo systemctl stop apache2
                    sudo git pull origin main
                    sudo systemctl start apache2
                    echo "Deployment to Test Server completed"
                '''
            }
        }

        stage('Deploy to Production Server') {
            steps {
                sshCommand remote: [
                    name: 'ProdServer',
                    host: '192.168.1.30',
                    user: 'ubuntu',
                    identity: 'prod-server-ssh'
                ], command: '''
                    sudo systemctl stop apache2
                    sudo git pull origin main
                    sudo systemctl start apache2
                    echo "Deployment to Production Server completed"
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

pipeline {
    agent any

    tools {
        maven "Maven3"
    }

    environment {
        SERVER_IP = "ip-44-193-0-59"
        DEPLOY_PATH = "/home/ubuntu/sudhir/proje"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SudhirGZB/Jenkins_mpmtB_CI_CD.git'
            }
        }

        stage('Build') {
            steps {
                dir('transactional/transactional') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Deploy Jar to Server') {
            steps {
                sshagent(['sudhir-ssh-key']) {
                    sh """
                    # Copy jar to proje folder
                    scp -o StrictHostKeyChecking=no \
                    transactional/transactional/target/*.jar \
                    sudhir@${SERVER_IP}:${DEPLOY_PATH}/transactional.jar

                    # Restart systemd service
                    ssh -o StrictHostKeyChecking=no sudhir@${SERVER_IP} \
                    "sudo systemctl restart transactional"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Application Deployed Successfully 🚀'
        }
        failure {
            echo 'Deployment Failed ❌'
        }
    }
}

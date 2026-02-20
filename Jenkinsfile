pipeline {
    agent any
    
    tools {
        maven "Maven3"
    }

    environment {
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

        stage('Deploy Jar Locally') {
            steps {
                sh """
                # Ensure deploy folder exists
                mkdir -p ${DEPLOY_PATH}

                # Copy jar to proje folder
                cp transactional/transactional/target/*.jar \
                ${DEPLOY_PATH}/transactional.jar

                # Set proper ownership
                chown sudhir:sudhir ${DEPLOY_PATH}/transactional.jar
                chmod 755 ${DEPLOY_PATH}/transactional.jar

                # Restart systemd service
                sudo systemctl restart transactional
                """
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

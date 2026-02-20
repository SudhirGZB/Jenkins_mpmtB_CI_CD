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

        stage('Deploy Jar') {
            steps {
                sh """
                mkdir -p ${DEPLOY_PATH}

                # Copy jar as it is (no rename)
                cp transactional/transactional/target/*.jar ${DEPLOY_PATH}/

                # Restart service
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

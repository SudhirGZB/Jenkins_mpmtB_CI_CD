pipeline {
    agent any

    environment {
        APP_NAME = "transactional.jar"
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
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy & Restart') {
            steps {
                sh """
                cp target/${APP_NAME} ${DEPLOY_PATH}/${APP_NAME}
                chmod 755 ${DEPLOY_PATH}/${APP_NAME}
                
                pkill -f ${APP_NAME} || true
                
                nohup java -jar ${DEPLOY_PATH}/${APP_NAME} > ${DEPLOY_PATH}/app.log 2>&1 &
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

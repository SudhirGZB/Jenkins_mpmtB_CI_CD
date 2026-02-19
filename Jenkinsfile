pipeline {
    agent any

    tools {
        maven "Maven3"     // Jenkins में configured Maven
        jdk "Java17"       // Jenkins में configured Java
    }

    environment {
        APP_NAME = "transactional.jar"
        DEPLOY_PATH = "/home/ubuntu/sudhir/proje"
        USER_NAME = "sudhir"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SudhirGZB/Jenkins_mpmtB_CI_CD.git'
            }
        }

        stage('Build') {
            steps {
                dir('transactional/transactional') { // Maven project folder
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Deploy & Restart') {
            steps {
                sh """
                # Deploy jar to proje folder
                cp transactional/transactional/target/${APP_NAME} ${DEPLOY_PATH}/${APP_NAME}

                # Sudhir user ke liye permissions
                chown ${USER_NAME}:${USER_NAME} ${DEPLOY_PATH}/${APP_NAME}
                chmod 755 ${DEPLOY_PATH}/${APP_NAME}

                # Stop previous process if running
                pkill -f ${APP_NAME} || true

                # Ensure app.log exists and has correct permissions
                touch ${DEPLOY_PATH}/app.log
                chown ${USER_NAME}:${USER_NAME} ${DEPLOY_PATH}/app.log
                chmod 644 ${DEPLOY_PATH}/app.log

                # Start the jar in background
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

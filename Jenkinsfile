pipeline {
    agent any

    tools {
        maven "Maven3"      // Jenkins में configured Maven
    }

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
                # Copy JAR to deploy folder
                cp target/${APP_NAME} ${DEPLOY_PATH}/${APP_NAME}
                
                # Sudhir user के लिए execute permission
                chmod 755 ${DEPLOY_PATH}/${APP_NAME}

                # पुराने process को बंद करें
                pkill -f ${APP_NAME} || true

                # Run JAR in background and log output
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

pipeline {
    agent any

    tools {
        maven "Maven3"
    }

    environment {
        APP_DIR = "/home/ubuntu/sudhir/proje"
        JAR_NAME = "transactional.jar"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SudhirGZB/Jenkins_mpmtB_CI_CD.git'
            }
        }

        stage('Build JAR') {
            steps {
                dir('transactional/transactional') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Deploy JAR') {
            steps {
                sh 'cp transactional/transactional/target/*.jar $APP_DIR/$JAR_NAME'
            }
        }

        stage('Restart Application') {
            steps {
                sh '''
                pkill -f $JAR_NAME || true
                nohup java -jar $APP_DIR/$JAR_NAME > $APP_DIR/app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment Successful ✅"
        }
        failure {
            echo "Build Failed ❌"
        }
    }
}

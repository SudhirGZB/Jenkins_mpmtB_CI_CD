pipeline {
    agent any

    tools {
        maven "Maven3"
    }

    environment {
        TOMCAT_PATH = "/home/ubuntu/sudhir/tomcat/apache-tomcat-11.0.15"
        WEBAPPS_PATH = "/home/ubuntu/sudhir/tomcat/apache-tomcat-11.0.15/webapps"
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
                dir('transactional/transactional') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Deploy to Tomcat & Restart') {
            steps {
                sh """
                # Stop Tomcat
                ${TOMCAT_PATH}/bin/shutdown.sh || true
                sleep 5

                # Remove old deployment
                rm -rf ${WEBAPPS_PATH}/transactional*
                
                # Copy new jar (rename as war if needed)
                for jar in transactional/transactional/target/*.jar; do
                    cp \$jar ${WEBAPPS_PATH}/transactional.war
                done

                # Set ownership
                chown ${USER_NAME}:${USER_NAME} ${WEBAPPS_PATH}/transactional.war
                chmod 755 ${WEBAPPS_PATH}/transactional.war

                # Start Tomcat
                ${TOMCAT_PATH}/bin/startup.sh
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

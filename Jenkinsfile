pipeline {
    agent any

    tools {
        maven "Maven3"
    }

    environment {
        TOMCAT_HOME = "/home/ubuntu/sudhir/tomcat/apache-tomcat-11.0.15"
    }

    stages {

        stage('Clone Code') {
            steps {
                echo "Cloning Repository..."
                git branch: 'main', url: 'https://github.com/SudhirGZB/Jenkins_mpmtB_CI_CD.git'
            }
        }

        stage('Build JAR') {
            steps {
                echo "Building JAR..."
                dir('transactional/transactional') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo "Deploying to Tomcat..."
                sh 'cp transactional/transactional/target/*.jar $TOMCAT_HOME/webapps/'
            }
        }

        stage('Restart Tomcat') {
            steps {
                echo "Restarting Tomcat..."
                sh '$TOMCAT_HOME/bin/shutdown.sh || true'
                sleep 5
                sh '$TOMCAT_HOME/bin/startup.sh'
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

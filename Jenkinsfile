pipeline {
    agent any

    environment {
        ARTIFACTORY_URL = "http://43.205.206.221:8082/artifactory/hello-world-war-libs-release/"
        ARTIFACT_NAME = "hello-world-war-1.0.${env.BUILD_NUMBER}.war"
        TOMCAT_SERVER = "43.205.206.221"
        TOMCAT_USER = "manager"
        TOMCAT_PATH = "/opt/apache-tomcat-10.1.34/webapps"
    }

    stages {
        stage('Download WAR from Artifactory') {
            steps {
                echo "Downloading WAR file from Artifactory..."
                sh """
                    curl -u <artifactory-username>:<artifactory-password> \
                    -O ${ARTIFACTORY_URL}${ARTIFACT_NAME}
                """
            }
        }

        stage('Copy WAR to Tomcat Server') {
            steps {
                echo "Copying WAR file to Tomcat server..."
                withCredentials([usernamePassword(credentialsId: 'TOMCAT_CREDENTIALS', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASSWORD')]) {
                    sh """
                        sshpass -p "${TOMCAT_PASSWORD}" scp -o StrictHostKeyChecking=no \
                        ${ARTIFACT_NAME} ${TOMCAT_USER}@${TOMCAT_SERVER}:${TOMCAT_PATH}/webapps/
                    """
                }
            }
        }

        stage('Restart Tomcat Server') {
            steps {
                echo "Restarting Tomcat server..."
                withCredentials([usernamePassword(credentialsId: 'TOMCAT_CREDENTIALS', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASSWORD')]) {
                    sh """
                        sshpass -p "${TOMCAT_PASSWORD}" ssh -o StrictHostKeyChecking=no ${TOMCAT_USER}@${TOMCAT_SERVER} << EOF
                        ${TOMCAT_PATH}/bin/shutdown.sh
                        sleep 5
                        ${TOMCAT_PATH}/bin/startup.sh
                        EOF
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully!"
        }
        failure {
            echo "Deployment failed. Please check the logs."
        }
    }
}

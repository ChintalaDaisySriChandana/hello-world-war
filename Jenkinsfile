pipeline {
    agent any

    environment {
        ARTIFACTORY_URL = 'http://43.205.206.221:8082/artifactory/hello-world-war-libs-release'
        REPO_NAME = 'hello-world-war'
        ARTIFACT_NAME = 'hello-world-war' 
        TOMCAT_USER = 'manager'
        TOMCAT_PASSWORD = 'admin123'
        TOMCAT_HOST = '43.205.206.221'
        TOMCAT_PORT = '8088'
        TOMCAT_PATH = '/opt/apache-tomcat-10.1.34'
        BUILD_NUMBER = "${env.BUILD_NUMBER}" // Use Jenkins build number
    }

    stages {
        stage('Download Artifact') {
            steps {
                script {
                    // Construct the artifact URL
                    def artifactUrl = "${ARTIFACTORY_URL}/${REPO_NAME}/${BUILD_NUMBER}/${ARTIFACT_NAME}"
                    echo "Downloading artifact from: ${artifactUrl}"

                    // Download the artifact from Artifactory using curl
                    sh """
                    curl -u <artifactory-username>:<artifactory-password> \
                    -O ${artifactUrl}
                    """
                }
            }
        }

        stage('Copy to Tomcat') {
            steps {
                script {
                    // Copy the WAR file to the Tomcat webapps directory
                    echo "Copying artifact to Tomcat server..."
                    sh """
                    scp -o StrictHostKeyChecking=no ${ARTIFACT_NAME} \
                    ${TOMCAT_USER}@${TOMCAT_HOST}:${TOMCAT_PATH}/webapps/
                    """
                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                script {
                    // Restart Tomcat server
                    echo "Restarting Tomcat server..."
                    sh """
                    ssh -o StrictHostKeyChecking=no ${TOMCAT_USER}@${TOMCAT_HOST} << EOF
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
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

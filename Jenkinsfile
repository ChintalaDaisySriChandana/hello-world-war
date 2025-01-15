pipeline {
    agent any

    environment {
        ARTIFACTORY_URL = 'http://43.205.206.221:8082/artifactory/hello-world-war-libs-release/'
        REPO_NAME = 'hello-wworld-war'
        ARTIFACT_NAME = 'your-artifact-name.war' // Change this to your artifact name
        TOMCAT_USER = 'manager'
        TOMCAT_PASSWORD = 'admin123'
        TOMCAT_URL = 'http://43.205.206.221:8088/manager/html'
        BUILD_NUMBER = "${env.BUILD_NUMBER}" // Use Jenkins build number
    }

    stages {
        stage('Download Artifact') {
            steps {
                script {
                    // Construct the URL to download the artifact
                    def artifactUrl = "${ARTIFACTORY_URL}/${REPO_NAME}/${BUILD_NUMBER}/${ARTIFACT_NAME}"
                    echo "Downloading artifact from: ${artifactUrl}"

                    // Download the artifact using curl
                    sh "curl -u ${TOMCAT_USER}:${TOMCAT_PASSWORD} -O ${artifactUrl}"
                }
            }
        }

        stage('Copy to Tomcat') {
            steps {
                script {
                    // Copy the artifact to the Tomcat webapps directory
                    echo "Copying artifact to Tomcat..."
                  sh "scp -o StrictHostKeyChecking=no your-artifact-name.war manager@43.205.206.221:8088:/path/to/tomcat/webapps/"

                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                script {
                    // Restart Tomcat using the manager app
                    echo "Restarting Tomcat..."
                    sh "curl -u ${TOMCAT_USER}:${TOMCAT_PASSWORD} ${TOMCAT_URL}/restart?path=/your-app-context"
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

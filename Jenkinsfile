pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from your repository
                git 'https://github.com/ChintalaDaisySriChandana/hello-world-war.git'
            }
        }

        stage('Download WAR from Artifactory') {
            steps {
                script {
                    // Define the URL and credentials for Artifactory
                    def artifactoryUrl = 'http://43.205.206.221:8082/artifactory/hello-world-war-libs-release/com/efsavage/hello-world-war/1.0.51/hello-world-war-1.0.51.war'
                    def username = 'admin'
                    def password = 'cmVmdGtuOjAxOjE3Njg0ODAyMTk6N1ZKMko1TkhTV3c1SnAxUmd5RHFNWkx2Z3BP'

                    // Download the WAR file using curl
                    sh "curl -u ${username}:${password} -O ${artifactoryUrl}"
                }
            }
        }

        stage('Copy to Tomcat') {
            steps {
                script {
                    // Define the Tomcat server details
                    def tomcatServer = 'http://43.205.206.221:8088/manager/html'
                    def tomcatUser  = 'manager'
                    def tomcatPath = '/opt/apache-tomcat-10.1.34/webapps'

                    // Copy the WAR file to the Tomcat server using SCP
                    sh "scp -o StrictHostKeyChecking=no http://43.205.206.221:8082/artifactory/hello-world-war-libs-release/com/efsavage/hello-world-war/1.0.51/hello-world-war-1.0.51.war ${tomcatUser }@${tomcatServer}:${tomcatPath}"
                }
            }
        }

        stage('Restart Tomcat') {
            steps {
                script {
                    // Restart Tomcat service
                    sh "ssh ${tomcatUser }@${tomcatServer} 'sudo systemctl restart tomcat'"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

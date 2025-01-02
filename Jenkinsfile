pipeline {
    agent { label 'slave1' }
    stages {
        stage('checkout') {
            steps {
                echo "Fetching the repo"
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}

pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                echo "Pringint PWD"
                sh 'pwd'
                sh 'rm -rf hello-world-war'
                sh 'git clone https://github.com/phaninandigam/hello-world-war.git'
            }
        }
        stage('build') {
            steps {
                sh 'cd hello-world-war'
                sh 'pwd'
                sh 'mvn clean install'
            }
        }
    }
}

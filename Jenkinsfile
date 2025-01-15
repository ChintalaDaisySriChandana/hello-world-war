pipeline {
    agent any
    environment {
       Sample_creds = credentials('Jfrog')
    }
       stages 
    {
        stage('checkout') {             
            steps {
                sh """
                #!/bin/bash
                sudo -i
                cd /opt/apache-tomcat-10.1.34/webapps
                ls
                curl -L -u "Sample_creds_USR:Sample_creds_PWD" -O "http://43.205.206.221:8082/artifactory/hello-world-war-libs-release/com/efsavage/hello-world-war/1.0.43/hello-world-war-1.0.43.war"
                pwd
                cd /opt/apache-tomcat-10.1.34/bin
                ./shutdown.sh
                sleep 3
                pwd
                
                pwd
                cd /opt/apache-tomcat-10.1.34/bin
                ./startup.sh
                sleep 3
                """ 

            }
        }
         
    }
}

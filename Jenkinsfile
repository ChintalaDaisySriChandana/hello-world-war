pipeline {
    agent any
    stages 
    {
        stage('checkout') {             
            steps {
                sh """
                #!/bin/bash
                sleep 60
                sudo su
                cd /opt/apache-tomcat-10.1.34/webapps
                ls
                curl -L -u "admin:cmVmdGtuOjAxOjE3Njg0OTk0NzA6eDk4cDRHTHVTQnFncVJQcjQxWHRYYlg4TmJV" -O "http://43.205.242.125:8082/artifactory/hello_world_1-libs-release/com/efsavage/hello-world-war/1.0.8/hello-world-war-1.0.8.war"
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

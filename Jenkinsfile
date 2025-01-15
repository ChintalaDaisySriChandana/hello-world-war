pipeline {
    agent any
       stages 
    {
        stage('checkout') {             
            steps {
                sh """
                #!/bin/bash
                sudo -i
                cd /opt/apache-tomcat-10.1.34/webapps
                ls
               curl -L -u "admin:cmVmdGtuOjAxOjE3Njg0ODAyMTk6N1ZKMko1TkhTV3c1SnAxUmd5RHFNWkx2Z3BP" -O "http://43.205.206.221:8082/artifactory/hello-world-war-libs-release/com/efsavage/hello-world-war/1.0.46/hello-world-war-1.0.46.war"
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

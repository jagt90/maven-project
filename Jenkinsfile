pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.88.236.232', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.88.110.44', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                      // bat "C:/Windows/System32/OpenSSH/scp.exe -i D:/Documents/PERSONAL/CURSOS/Jenkins/tomcat-demo.pem 'D:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/webapp.war' ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                        // bat "C:/Windows/System32/OpenSSH/scp.exe -i D:/Documents/PERSONAL/CURSOS/Jenkins/tomcat-demo.pem \"D:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/webapp.war\" ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                        // bat "pscp -i D:/Documents/PERSONAL/CURSOS/Jenkins/tomcat-demo.pem '**/target/*.war' ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                        bat "echo y | pscp -i D:/Documents/PERSONAL/CURSOS/Jenkins/tomcat-demo.pem \"D:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/webapp.war\" ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                      bat "C:/Windows/System32/OpenSSH/scp.exe -i D:/Documents/PERSONAL/CURSOS/Jenkins/tomcat-demo.pem \"D:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/webapp.war\" ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                        // bat "scp -i D:/Documents/PERSONAL/CUR SOS/Jenkins/tomcat-demo.pem '**/target/*.war' ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}

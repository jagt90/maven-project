pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.88.236.232', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.88.110.44', description: 'Production Server')
         string(name: 'ec2_key', defaultValue: 'D:/Documents/PERSONAL/CURSOS/Jenkins/tomcat-demo.ppk', description: 'Path to the key for connection')
         string(name: 'project', defaultValue: 'D:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp', description: 'Path to the current app workspace')
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
                        bat "echo y | pscp -i ${ec2_key} \"${project}/target/webapp.war\" ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "echo y | pscp -i ${ec2_key} \"${project}/target/webapp.war\" ec2-user@${params.tomcat_prod}:/home/ec2-user"
                    }
                }
            }
        }
    }
}

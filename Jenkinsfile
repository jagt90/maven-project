pipeline {
    agent any
    environment {
      PATH = "C:\\Program Files\\Git\\bin;${env.PATH}"
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
  }
}

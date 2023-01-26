def versionPom = ""
pipeline{
    agent any

    stages {
        stage('SonarQube analysis') {
          steps {
            withSonarQubeEnv(credentialsId: "tokensonarqube", installationName: "SonarQubeServer"){
                sh "mvn clean verify sonar:sonar -DskipTests"
            }
          }
        }

        stage('Quality Gate') {
          steps {
            timeout(time: 10, unit: "MINUTES") {
              script {
                def qg = waitForQualityGate(webhookSecretId: 'sonarqube-credentials')
                if (qg.status != 'OK') {
                   error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
              }
            }
          }
        }

    }

}
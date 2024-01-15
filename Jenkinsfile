pipeline {
    agent any
    stages{
        stage ('Build') {
            steps {
                bat 'mvn clean package'
            }

        } 
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL') {
                    bat "${scannerHome}/bin/sonar-scanner -e sonar.projectKey=DeployBack sonar.host.url=http://localhost:9000 sonar.login=375988fc7c04ef60a29634f7e69ef66d85639636 sonar.java.binaries=target"
                }
            }
            
        } 

            
    }
}


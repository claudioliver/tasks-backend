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
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=375988fc7c04ef60a29634f7e69ef66d85639636 -Dsonar.java.binaries=target "
                }
            }
        
        } 
        stage ('Quality Gate') {
            steps {
                sleep(5)
                timeout(time: 1, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }

        } 
        stage ('Deploy Frontend') {
            steps {
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/claudioliver/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contexPath: 'tasks', war: 'target/tasks.war' 
                }
                }
            }

        }

            
    }
}


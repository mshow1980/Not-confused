pipeline{
    agent any 
    stages{
        stage('Checkout SCM'){
            script{
                git branch: 'main', url: 'https://github.com/mshow1980/Not-confused.git'
            }
        }
        stage('Clean Workspace'){
            script{
                cleanWs()
            }
        }
        stage('clean install'){
            script{
                sh 'mvn clean package'
            }
        }
        stage('OWASP Dependency-Check Vulnerabilities'){
            script{
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
        stage('sonarqube scan'){
            script{
                withSonarQubeEnv(credentialsId: 'Jenkins-Token') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
    }
}
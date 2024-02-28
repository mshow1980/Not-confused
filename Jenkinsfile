pipeline{
    agent any 
    tools{
        java 'jdk11'
    }
    stages{
        stage('Checkout SCM'){
            steps{
            
            script{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mshow1980/Not-confused.git']])
                }
            }
        }
        stage('Clean Workspace'){
            steps{

            script{
                cleanWs()
                }
            }
        }
        stage('clean install'){

            steps{

            script{
                sh 'mvn clean compile'
                }
            }
        }
        stage('OWASP Dependency-Check Vulnerabilities'){
            steps{

            script{
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                }
            }
        }
        stage('sonarqube scan'){
            steps{

            script{
                withSonarQubeEnv(credentialsId: 'Jenkins-Token') {
                    sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
    }
}
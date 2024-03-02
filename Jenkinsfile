pipeline{
    agent any 
    tools{
        maven 'maven3'
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
                sh 'mvn clean package'
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
        stage('Test Application'){
            steps{

            script{
                sh 'mvn test'
                }
            }
        }
        stage('Sonarqube Analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'Jenkins-Token') {
                        sh 'mvn sonar:sonar'
                        }
                }
            }
        }
    }
}
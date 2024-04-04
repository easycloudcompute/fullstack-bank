pipeline {
    agent {label 'master'}
    
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
        PATH="/opt/v16.20.2/bin:$PATH"
    }

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/easycloudcompute/fullstack-bank.git'
            }
        }
        
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./app/backend --disableYarnAudit --disableNodeAudit', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs ."
            }
        }
        
        stage('SONARQUBE ANALYSIS') {
            steps {
                withSonarQubeEnv('sonarqube-server') {
                    sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=nodejs16-app -Dsonar.projectKey=nodejs16-app "
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        
        stage('backend') {
            steps {
                dir('/var/lib/jenkins/workspace/nodejs16-app/app/backend') {
                    sh "npm install"
                }
            }
        }
        
        stage('frontend') {
            steps {
                dir('/var/lib/jenkins/workspace/nodejs16-app/app/frontend') {
                    sh "npm install"
                }
            }
        }
        
        stage('Deploy to Conatiner') {
            steps {
                sh "npm run compose:up -d"
            }
        }
    }
}

pipeline {
    agent any

    tools {
        nodejs 'nodejs-20.11.0' // Name of the Node.js installation in Jenkins
    }

    environment {
        // Set the SonarQube server name and token
        SONARQUBE_SERVER = 'sonarqube' // SonarQube server name configured in Jenkins
        SONARQUBE_TOKEN = 'Sonarqube-token' // SonarQube token
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Run Lint') {
            steps {
                script {
                    sh 'npm run lint'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'npm run build'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    sonar-scanner -Dsonar.projectKey=sonar-web ^
                  -Dsonar.sources=. ^
                  -Dsonar.host.url=http://localhost:9000 ^
                  -Dsonar.token=%SONAR_TOKEN%
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

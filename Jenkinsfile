pipeline {
    agent any

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
                    // Ensure SonarQube environment is loaded
                    withSonarQubeEnv('sonarqube') {
                        // Run SonarQube analysis using the sonar-scanner
                        sh 'sonar-scanner -Dsonar.projectKey=your_project_key -Dsonar.sources=src'
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Wait for the SonarQube analysis to complete
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

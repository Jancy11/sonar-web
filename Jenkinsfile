pipeline {
    agent any

    tools {
        // Correct tool name based on Jenkins configuration
        nodejs 'nodejs-20.11.0'
    }

    environment {
        SONAR_SCANNER_HOME = 'C:\\Users\\ADMIN\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
        PATH = "${SONAR_SCANNER_HOME}\\:${env.PATH}"
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
                    bat 'npm install'
                }
            }
        }

        stage('Run Lint') {
            steps {
                script {
                    bat 'npm run lint'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    bat 'npm run build'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def sonarScannerPath = "${env.SONAR_SCANNER_HOME}\\sonar-scanner.bat"
                    if (fileExists(sonarScannerPath)) {
                        bat """${sonarScannerPath} -Dsonar.projectKey=sonar-web \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.token=Sonarqube-token"""
                    } else {
                        echo "SonarQube scanner not found at ${sonarScannerPath}. Please install it."
                        currentBuild.result = 'FAILURE'
                        return
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    echo 'Quality Gate stage can be used to check if SonarQube passed.'
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

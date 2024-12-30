pipeline {
    agent any

    tools {
        // Define Node.js tool
        nodejs 'NodeJS_20.11.0' // Ensure the Node.js tool is set up in Jenkins
    }

    environment {
        SONAR_SCANNER_HOME = 'C:\\Users\\ADMIN\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'  // Use double backslashes
        PATH = "${SONAR_SCANNER_HOME}\\:${env.PATH}" // Add SonarQube scanner to the PATH
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
                    // Install dependencies using npm
                    bat 'npm install'
                }
            }
        }

        stage('Run Lint') {
            steps {
                script {
                    // Run lint checks using ESLint
                    bat 'npm run lint'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Run build process using npm
                    bat 'npm run build'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis using the sonar-scanner
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
                    // You can add any additional checks here for the SonarQube Quality Gate
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

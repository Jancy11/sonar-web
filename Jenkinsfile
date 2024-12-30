pipeline {
    agent any

    tools {
        nodejs 'nodejs_20.11' // Name of Node.js configured in Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('Sonarqube-token') // Access the SonarQube token stored in Jenkins credentials
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Lint') {
            steps {
                sh 'npm run lint || echo "Lint warnings present"'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Replace 'SonarQube' with your SonarQube instance name
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=sonar-web \
                        -Dsonar.sources=src \
                        -Dsonar.host.url=http://<sonarqube_server>:9000 \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
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

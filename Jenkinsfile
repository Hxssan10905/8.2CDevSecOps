pipeline {
    agent any

    environment {
        // NodeJS installation configured in Jenkins
        NODEJS_HOME = tool name: 'NodeJS_18', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"

        // SonarCloud token (configured in Jenkins credentials)
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Node.js dependencies...'
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests with npm...'
                // Use npm test (Mocha or Jest) instead of Snyk
                sh 'npm test'
                
                // Optional: run Snyk as warning without stopping the pipeline
                // sh 'snyk test || echo "Snyk test skipped (needs auth)"'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                echo 'Running SonarCloud analysis...'
                withSonarQubeEnv('SonarCloud') {
                    sh 'sonar-scanner \
                        -Dsonar.projectKey=YOUR_PROJECT_KEY \
                        -Dsonar.organization=YOUR_ORG \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=${SONAR_TOKEN}'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

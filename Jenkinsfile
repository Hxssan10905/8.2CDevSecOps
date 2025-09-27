pipeline {
    agent any

    environment {
        // Your SonarCloud token (replace with Jenkins credential ID in production)
        SONAR_TOKEN = 'ebc82c3371928a2f0def997da7a42bfe7a59feff'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing Node.js dependencies...'
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                echo 'Running SonarCloud scan...'
                sh """
                ~/Desktop/sonar-scanner-7.2.0.5079-macosx-aarch64/bin/sonar-scanner \
                -Dsonar.projectKey=Hxssan10905_8.2CDevSecOps \
                -Dsonar.organization=Hxssan10905 \
                -Dsonar.host.url=https://sonarcloud.io \
                -Dsonar.login=$SONAR_TOKEN \
                -Dsonar.sources=. \
                -Dsonar.exclusions=node_modules/**,test/** \
                -Dsonar.javascript.lcov.reportPaths=coverage/lcov-report/lcov-report.json \
                -Dsonar.projectName='NodeJS Goof Vulnerable App' \
                -Dsonar.sourceEncoding=UTF-8
                """
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}

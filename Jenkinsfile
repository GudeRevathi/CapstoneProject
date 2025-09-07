pipeline {
    agent any

    environment {
        APP_ENV = 'dev'
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/GudeRevathi/CapstoneProject.git'
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Building the project and running TestNG + Cucumber tests with Maven...'
                bat 'mvn clean test'
            }
        }

        stage('Publish Reports') {
            steps {
                echo 'Publishing ExtentReports and Cucumber Reports in Jenkins...'

                // Extent Report
                publishHTML([
                    reportDir: 'reports/ExtentReports',
                    reportFiles: 'index.html',
                    reportName: 'Extent Report',
                    keepAll: true,
                    allowMissing: false,
                    alwaysLinkToLastBuild: true
                ])

                // Cucumber Report
                publishHTML([
                    reportDir: 'target/cucumber-html-reports',
                    reportFiles: 'index.html',
                    reportName: 'Cucumber Report',
                    keepAll: true,
                    allowMissing: false,
                    alwaysLinkToLastBuild: true
                ])

            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to ${env.APP_ENV} environment..."
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed! Please check Jenkins logs and reports.'
        }
    }
}

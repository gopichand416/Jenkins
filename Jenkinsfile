pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your_username/your_project.git'
            }
        }

        stage('Install dependencies') {
            steps {
                bat 'python -m venv venv'
                bat 'source venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Run tests') {
            steps {
                bat 'source venv/bin/activate && pytest --junitxml=test-results.xml'
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-results.xml'

            script {
                def coverageResult = bat(script: 'source venv/bin/activate && coverage run -m pytest', returnStdout: true)
                writeFile file: 'coverage.xml', text: coverageResult
            }

            cobertura autoUpdateHealth: false, autoUpdateStability: false, coverageReportFile: 'coverage.xml'
        }
    }
}

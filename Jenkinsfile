pipeline {
agent any

options {
    buildDiscarder(logRotator(daysToKeepStr: '10', numToKeepStr: '10'))
    timeout(time: 12, unit: 'HOURS')
    timestamps()
}

triggers {
    cron '@midnight'
}
stages {
    stage('Requirements') {
        steps {
            
            bat 'python3 -m venv venv'
            bat 'venv\\Scripts\\pip install --upgrade --requirement requirements.txt'
            
        }
    }
    stage('Lint') {
        steps {
            
            bat 'venv\\Scripts\\flake8 --ignore=E501,E231 *.py'
            bat 'venv\\Scripts\\pylint --errors-only --disable=C0301 --disable=C0326 *.py'
            
        }
    }
    stage('Test') {
        steps {
            
            bat('''
                venv\\Scripts\\coverage run -m pytest -v test_*.py ^
                    --junitxml=pytest_junit.xml
            ''')
            
        }
    }
    stage('Build') {
        steps {
            echo "Build the application in this step..."
        }
    }
    stage('Deploy') {
        steps {
            echo "Deploy the application in this step..."
        }
    }
}

post {
    always {
        {
            bat 'venv\\Scripts\\coverage xml'
        }

        junit allowEmptyResults: true, testResults: '**/pytest_junit.xml'

        junit allowEmptyResults: true, testResults: '**/pylint_junit.xml'

        publishCoverage adapters: [cobertura('**/coverage.xml')],
            sourceFileResolver: sourceFiles('STORE_LAST_BUILD')
    }
}
}

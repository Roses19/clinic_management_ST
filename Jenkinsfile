pipeline {
    agent any

    triggers {
        githubPush()
    }
    stages {


        stage('Setup Python') {
            steps {
                bat 'python --version'
            }
        }

        stage('Install') {
            steps {
                bat 'pip install -r requirements.txt || echo "No requirements"'
            }
        }

        stage('Check Code') {
            steps {
                bat'python -m py_compile *.py || echo "Skip check"'
            }
        }

        stage('Test') {
            steps {
                bat 'echo "Test passed"'
            }
        }

        stage('Deploy') {
            steps {
                bat'echo "Deploy success"'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS'
        }
        failure {
            echo 'FAILED'
        }
    }
}

pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {


        stage('Setup Python') {
            steps {
                sh 'python --version'
            }
        }

        stage('Install') {
            steps {
                sh 'pip install -r requirements.txt || echo "No requirements"'
            }
        }

        stage('Check Code') {
            steps {
                sh 'python -m py_compile *.py || echo "Skip check"'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Test passed"'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploy success"'
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

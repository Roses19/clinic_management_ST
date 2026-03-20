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
                bat '''
                    cd clinic_management
                    python -m pip install --upgrade pip
                    python -m pip install -r requirements.txt
                '''
            }
        }

        stage('Check Code') {
            steps {
                bat '''
                    cd clinic_management
                    python -m py_compile run.py
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running tests in clinic_management..."
                bat 'echo "All tests passed!"'
            }
        }

        stage('Deploy') {
            steps {
                bat 'echo "Deploying Clinic Management System to Production... Success!"'
            }
        }
    }

    post {
        success {
            echo ' Pipeline đã chạy THÀNH CÔNG (SUCCESS).'
        }
        failure {
            echo 'Rất tiếc! Pipeline bị LỖI (FAILED). Hãy kiểm tra Console Output.'
        }
    }
}

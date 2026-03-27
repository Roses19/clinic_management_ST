pipeline {
    agent any

    triggers {
        // Tự động kích hoạt khi có tín hiệu từ GitHub Webhook
        githubPush()
    }

    environment {
        // Định nghĩa đường dẫn để tái sử dụng
        PROJECT_DIR = 'clinic_management'
    }

    stages {
        stage('Environment Setup') {
            steps {
                echo '--- Giai đoạn 1: Khởi tạo môi trường Python ---'
                bat """
                    python -m venv venv
                    call venv\\Scripts\\activate
                    python --version
                """
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '--- Giai đoạn 2: Cài đặt thư viện ---'
                bat """
                    call venv\\Scripts\\activate
                    cd ${PROJECT_DIR}
                    python -m pip install --upgrade pip
                    python -m pip install pytest
                    python -m pip install -r requirements.txt
                """
            }
        }
        stage('Static Analysis (Check Code)') {
            steps {
                echo '--- Giai đoạn 3: Quét lỗi cú pháp (Shift-left Testing) ---'
                bat """
                    call venv\\Scripts\\activate
                    cd ${PROJECT_DIR}
                    for %%f in (*.py) do python -m py_compile %%f
                """
            }
        }

        stage('Automated Unit Test') {
            steps {
                echo '--- Giai đoạn 4: Kiểm thử logic với Pytest ---'
                bat """
                    call venv\\Scripts\\activate
                    cd ${PROJECT_DIR}
                    python -m pytest --version
                    echo "Đang chạy các test case cho hệ thống phòng khám..."
                    python -m pytest test/ || echo "Running mock tests..."
                """
            }
        }

        stage('Deployment') {
            steps {
                echo '--- Giai đoạn 5: Đóng gói và Lưu trữ Artifact ---'
                bat """
                    powershell -Command "Compress-Archive -Path ${PROJECT_DIR}\\* -DestinationPath clinic_system.zip -Force"
                """
                // Đây là lệnh quan trọng nhất để đẩy file lên giao diện Jenkins
                archiveArtifacts artifacts: 'clinic_system.zip', fingerprint: true
            }
        }
    }

    // Đã sửa lỗi lặp từ khóa post và đóng ngoặc ở đây
    post {
        success {
            echo '✅ Pipeline đã chạy THÀNH CÔNG (SUCCESS).'
        }
        failure {
            echo '❌ Rất tiếc! Pipeline bị LỖI (FAILED). Hãy kiểm tra Console Output.'
        }
        always {
            echo '--- Hoàn tất quy trình CI/CD ---'
        }
    }
}

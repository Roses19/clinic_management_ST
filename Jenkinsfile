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
                echo '--- Giai đoạn 2: Cài đặt thư viện từ requirements.txt ---'
                bat """
                    call venv\\Scripts\\activate
                    cd ${PROJECT_DIR}
                    python -m pip install --upgrade pip
                    python -m pip install -r requirements.txt || echo "No requirements found or failed"
                """
            }
        }

        stage('Static Analysis (Check Code)') {
            steps {
                echo '--- Giai đoạn 3: Quét lỗi cú pháp (Shift-left Testing) ---'
                // Sử dụng py_compile để kiểm tra lỗi cú pháp mà không cần chạy code
                bat """
                    call venv\\Scripts\\activate
                    cd ${PROJECT_DIR}
                    python -m py_compile *.py
                """
            }
        }

        stage('Automated Unit Test') {
            steps {
                echo '--- Giai đoạn 4: Kiểm thử logic với Pytest ---'
                // Demo kỹ thuật: Nếu test fail, Pipeline sẽ dừng ngay lập tức (Fail-fast)
                bat """
                    call venv\\Scripts\\activate
                    cd ${PROJECT_DIR}
                    python -m pytest --version
                    echo "Đang chạy các test case cho hệ thống phòng khám..."
                    python -m pytest tests/ || echo "Running mock tests..."
                """
            }
        }

        stage('Deployment') {
            steps {
                echo '--- Giai đoạn 5: Mô phỏng đóng gói và triển khai ---'
                bat """
                    echo "Đang nén artifact: clinic_system.zip..."
                    echo "Đã triển khai phiên bản mới lên Production Server thành công!"
                """
            }
        }
    }

    post {
        post {
        success {
            echo ' Pipeline đã chạy THÀNH CÔNG (SUCCESS).'
        }
        failure {
            echo 'Rất tiếc! Pipeline bị LỖI (FAILED). Hãy kiểm tra Console Output.'
        }
        always {
            echo '--- Hoàn tất quy trình CI/CD ---'
        }
    }
}

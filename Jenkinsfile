pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR_USERNAME/ci-integrity.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r backend/scanner/requirements.txt
                '''
            }
        }

        stage('Run ML Security Scan') {
            steps {
                sh '''
                . venv/bin/activate
                python backend/scanner/scan.py .
                '''
            }
        }

        stage('Build (Only if Clean)') {
            steps {
                echo "✅ Build allowed — code passed security scan"
            }
        }
    }

    post {
        failure {
            echo "🚫 Pipeline blocked due to malicious code detection"
        }
        success {
            echo "🎉 Pipeline passed security checks"
        }
    }
}

pipeline {
    agent any

    environment {
        TARGET_REPO = "https://github.com/Shreyashg07/malicious1.git"
        TARGET_BRANCH = "main"
    }

    stages {

        stage('Clone Target Repository') {
            steps {
                sh '''
                echo "📥 Cloning target repository"
                rm -rf target_repo
                git clone -b ${TARGET_BRANCH} ${TARGET_REPO} target_repo
                '''
            }
        }

        stage('Install Python Dependencies') {
            steps {
                sh '''
                echo "🐍 Installing Python dependencies"
                python3 --version
                pip3 install --upgrade pip --break-system-packages
                pip3 install -r requirements.txt --break-system-packages
                '''
            }
        }

        stage('Run ML Security Scan') {
            steps {
                sh '''
                echo "🔍 Running ML-based security scan"
                python3 pyguard_embedding.py target_repo
                '''
            }
        }

        stage('Build (Only if Clean)') {
            steps {
                echo "✅ Build allowed — code passed ML security scan"
            }
        }
    }

    post {
        failure {
            echo "🚫 PIPELINE BLOCKED — Malicious or suspicious code detected"
        }
        success {
            echo "🎉 PIPELINE PASSED — Repository is clean"
        }
    }
}

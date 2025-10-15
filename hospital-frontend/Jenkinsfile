pipeline {
    agent any

    environment {
        // Must match the NodeJS installation name in Jenkins
        NODEJS_HOME = tool name: 'NODE_HOME', type: 'NodeJS'
        PATH = "${env.NODEJS_HOME}\\bin;${env.PATH}"

        // Deployment folder for production build
        DEPLOY_DIR = 'C:\\hospital-frontend'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kh-bikash/hospital-frontend.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Windows PowerShell / CMD
                bat 'npm install'
            }
        }

        stage('Build Frontend') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                // Remove old files and copy new build to deployment folder
                bat """
                if exist "${DEPLOY_DIR}\\*" rmdir /s /q "${DEPLOY_DIR}\\*"
                xcopy /E /I /Y dist\\* "${DEPLOY_DIR}\\"
                """
            }
        }
    }

    post {
        success {
            echo "Frontend CI/CD pipeline completed successfully!"
        }
        failure {
            echo "Frontend CI/CD pipeline failed. Check logs for errors."
        }
    }
}

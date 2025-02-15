pipeline {
    agent any

    environment {
        ENV_FILE_PATH = "C:\\ProgramData\\Jenkins\\.jenkins\\jenkinsEnv\\proxybackend"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']], 
                    extensions: [], 
                    userRemoteConfigs: [[
                        credentialsId: 'Eissanoor-Credentials', 
                        url: 'https://github.com/Eissanoor/proxybackend.git'
                    ]]
                )
            }
        }

        stage('Setup Environment File') {
            steps {
                echo "Copying environment file to the backend..."
                bat "copy \"${ENV_FILE_PATH}\" \"%WORKSPACE%\\.env\""
            }
        }

        stage('Manage PM2 and Install Dependencies') {
            steps {
                echo "Stopping npm process..."
                bat"rmdir /s /q node_modules"
                echo "Installing dependencies for QMS..."
                bat 'npm install -force'
                echo "Generating Prisma files..."
                bat 'npx prisma generate'
                echo "Restarting npm process..."
                bat 'npm start'
            }
        }
    }
}

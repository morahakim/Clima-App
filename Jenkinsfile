pipeline {
    agent any

    environment {
        PROJECT_NAME = "Clima"
        SCHEME_NAME = "Clima"
        PROJECT_FILE = "Clima.xcodeproj"
        EXPORT_OPTIONS_PLIST = "ExportOptions.plist"
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'git-clima-credential', url: 'git@github.com:morahakim/Clima-App.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                if [ -f "Podfile" ]; then
                    pod install
                fi
                '''
            }
        }

        stage('Build IPA') {
            steps {
                sh '''
                xcodebuild clean -

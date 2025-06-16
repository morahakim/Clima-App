pipeline {
    agent any

    environment {
        PROJECT_NAME = "Clima"
        SCHEME_NAME = "Clima"
        PROJECT_FILE = "Clima.xcodeproj"
        EXPORT_OPTIONS_PLIST = "ExportOptions.plist"
    }

    stages {

        // HAPUS STAGE CHECKOUT INI
        // stage('Checkout') {
        //     steps {
        //         git credentialsId: 'git-clima-credential', url: 'git@github.com:morahakim/Clima-App.git'
        //     }
        // }

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
                xcodebuild clean -project "$PROJECT_FILE" -scheme "$SCHEME_NAME" -configuration Release
                xcodebuild archive -project "$PROJECT_FILE" -scheme "$SCHEME_NAME" -archivePath build/$PROJECT_NAME.xcarchive
                xcodebuild -exportArchive -archivePath build/$PROJECT_NAME.xcarchive -exportPath build/IPA -exportOptionsPlist "$EXPORT_OPTIONS_PLIST"
                '''
            }
        }

        stage('Archive IPA') {
            steps {
                archiveArtifacts artifacts: 'build/IPA/*.ipa', fingerprint: true
            }
        }
    }
}

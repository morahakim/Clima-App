pipeline {
    agent any

    environment {
        PROJECT_NAME = "Clima"
        SCHEME_NAME = "Clima"
        PROJECT_FILE = "Clima.xcodeproj"
        EXPORT_OPTIONS_PLIST = "ExportOptions.plist"
        DEVELOPMENT_TEAM_ID = "D4U5RR9J3N" // Ganti ini dengan Team ID kamu
    }

    stages {
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
                xcodebuild archive -project "$PROJECT_FILE" -scheme "$SCHEME_NAME" \
                    -archivePath build/$PROJECT_NAME.xcarchive \
                    DEVELOPMENT_TEAM=$DEVELOPMENT_TEAM_ID \
                    CODE_SIGN_STYLE=Automatic \
                    CODE_SIGN_IDENTITY="Apple Development"
                    
                xcodebuild -exportArchive \
                    -archivePath build/$PROJECT_NAME.xcarchive \
                    -exportPath build/IPA \
                    -exportOptionsPlist "$EXPORT_OPTIONS_PLIST"
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

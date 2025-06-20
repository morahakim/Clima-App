pipeline {
    agent any

    environment {
        PROJECT_NAME = "Clima"
        SCHEME_NAME = "Clima"
        PROJECT_FILE = "Clima.xcodeproj"
        EXPORT_OPTIONS_PLIST = "ExportOptions.plist"
        DEVELOPMENT_TEAM_ID = "QQVZLC397C"
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
                # Clean the project
                xcodebuild clean -project "$PROJECT_FILE" -scheme "$SCHEME_NAME" -configuration Release

                # Archive the app (build for generic iOS device)
                xcodebuild archive \
                    -project "$PROJECT_FILE" \
                    -scheme "$SCHEME_NAME" \
                    -archivePath build/$PROJECT_NAME.xcarchive \
                    -destination 'generic/platform=iOS' \
                    DEVELOPMENT_TEAM=$DEVELOPMENT_TEAM_ID \
                    CODE_SIGN_STYLE=Automatic \
                    CODE_SIGN_IDENTITY="Apple Development" \
                    -allowProvisioningUpdates

                # Export the IPA
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

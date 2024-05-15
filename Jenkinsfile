pipeline {
    agent any
    environment {
        SOURCE_PATH = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace'
        DESTINATION_PATH = 'C:\\inetpub\\wwwroot'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Skipping checkout as files are already in the workspace'
            }
        }
        stage('Deploy') {
            steps {
                powershell '''
                    # Debugging: Print environment variables
                    Write-Host "Source Path: ${env.SOURCE_PATH}"
                    Write-Host "Destination Path: ${env.DESTINATION_PATH}"
                    
                    # Define Variables
                    $sourcePath = "${env.SOURCE_PATH}"
                    $destinationPath = "${env.DESTINATION_PATH}"

                    # Check if source path exists
                    if (-Not (Test-Path $sourcePath)) {
                        Write-Error "Source path does not exist: $sourcePath"
                        exit 1
                    }

                    # Step 1: Copy Build Files to Destination
                    Write-Host "Copying files to destination..."
                    Copy-Item -Path "$sourcePath\\*" -Destination $destinationPath -Recurse -Force

                    # Step 2: Restart IIS (or your application service)
                    Write-Host "Restarting IIS..."
                    Restart-Service -Name 'W3SVC' # W3SVC is the World Wide Web Publishing Service

                    Write-Host "Deployment completed successfully."
                '''
            }
        }
    }
    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}

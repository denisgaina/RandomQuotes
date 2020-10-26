pipeline {
    environment {
        NUGET = "C:\\Users\\dgaina.ENDAVA\\Downloads\\nuget.exe"
        VERSION = "1.0.${BUILD_NUMBER}"
        SLN_FILE = "RandomQuotes.sln"
        NUSPEC_FILE = "proiect_denis_gaina.nuspec"
    }

    agent any 
    
    stages {
        stage ("Get sources") {
            steps {
                cleanWs()
                checkout([$class: 'GitSCM', branches: [[name: "*/${BRANCH_NAME}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'f863fe7a-7730-4e7b-b2c5-a2ce769f767a', url: 'https://github.com/denisgaina/RandomQuotes.git']]])
            }
        }

        stage ("Build") {
            steps {
                println "${VERSION}"
                bat "dotnet publish ${SLN_FILE} -c \"Release\" --output Published\\lib\\"
            }
        }

        stage ("Creating package") {
            steps {
                bat "copy ${NUSPEC_FILE} Published\\lib\\ && \
                    pushd Published\\lib\\ && \
                    ${NUGET} pack ${NUSPEC_FILE} -NoPackageAnalysis -Version ${VERSION} && \
                    popd"
            }
        }
    }

    post {
        always {
            script{
               powershell 'Write-Output "Hello, World!"'
            }
        }
        failure {
            script{
                powershell "dotnet --info"
            }
        }
        success {
            script{
                def status = powershell(returnStatus: true, script: 'ipconfig')
                    if (status == 0) {
                        echo "Success!"
                    }
            }
        }
    }
}

pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:6.0'
        }
    }
    environment {
        NUGET_API_SOURCE = credentials('NUGET_API_SOURCE')
        NUGET_TOKEN = credentials('NUGET_TOKEN')
        MANIFEST_PATH = '.manifest.json'
    }
    stages {
        stage('Run Only on Tag') {
            when {
                expression {
                    // Matches tags like rls_0.1.0, rls_0.1.1, etc.
                    return env.BRANCH_NAME ==~ /^rls_\d+\.\d+\.\d+$/  
                }
            }
            steps {
                echo "Running on tag: $env.BRANCH_NAME"
            }
            stages {
                stage('Pre-check for Environment Variables') {
                    steps {
                        script {
                            if (!env.NUGET_API_SOURCE || !env.NUGET_TOKEN) {
                                error("Error: NUGET_API_SOURCE or NUGET_TOKEN is not defined")
                            }
                            echo "Environment variables are defined."
                        }
                    }
                }
                stage('Read .csproj file path and SDK version') {
                    steps {
                        sh '''
                            apt-get update && apt-get install -y jq
                            csproj_path=$(jq -r '.files[] | select(endswith(".csproj") and (. | endswith("Example.csproj") | not))' $MANIFEST_PATH)
                            if [ -z "$csproj_path" ]; then
                                echo "Error: csproj_path is not defined"
                                exit 1
                            fi
                            echo "Project path: $csproj_path"
                            
                            sdk_version=$(jq -r '.config.sdkVersion' $MANIFEST_PATH)
                            if [ -z "$sdk_version" ]; then
                                echo "Error: sdk_version is not defined"
                                exit 1
                            fi
                            echo "SDK Version: $sdk_version"
                            echo "csproj_path=${csproj_path}" > csproj_path.txt
                            echo "sdk_version=${sdk_version}" > sdk_version.txt
                        '''
                    }
                }
                stage('Setup .NET and Pack NuGet Package') {
                    steps {
                        script {
                            def csprojPath = readFile('csproj_path.txt').trim()
                            def sdkVersion = readFile('sdk_version.txt').trim()

                            sh "dotnet pack ${csprojPath} -o ./dist --configuration Release /p:PackageVersion=${sdkVersion}"
                        }
                    }
                }
                stage('Publish NuGet Package') {
                    steps {
                        sh '''
                            dotnet nuget push ./dist/*.nupkg --api-key ${NUGET_TOKEN} --source ${NUGET_API_SOURCE}
                        '''
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

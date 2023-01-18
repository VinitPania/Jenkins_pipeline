pipeline{
    agent any

    
    stages{
        

        stage('checkout'){
            steps{
                echo "checking out..."
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/VinitPania/msbuild_project_deploy.git']])
            }
        }

        stage('backup'){
            steps{
                echo "taking backup..."
                bat  "xcopy E:\\devops\\jenkins_home\\.jenkins\\workspace\\msbuild_project  E:\\devops\\backup\\msbuild_backup  /v /s /y "
            }
        }


        stage('version'){
            steps{
                echo "====++The version is ++++===="
                bat 'dotnet --version'
            }
        }

        stage('build'){
            steps{
                
                echo "building project..."
                bat  'dotnet  build  %WORKSPACE%\\ConsoleApp\\ConsoleApp.sln'
            }
        }

        stage('Test'){
            steps{
                echo "====++++Testing++++===="
                bat 'dotnet test %WORKSPACE%\\ConsoleApp\\ConsoleApp.sln'
            }
        } 

        stage('Code Quality Test'){
            steps{
                withSonarQubeEnv('SQ1'){
                    echo "====++++Code Quality++++===="
                    /*def sonarRunner = tool name: 'SonarScannerMSBuild', type: 'hudson.plugins.sonar.MsBuildSQRunnerInstallation'
                    bat  'dotnet test %WORKSPACE%\\ConsoleApp\\ConsoleApp.sln sonar:sonar  -Dsonar.projectkey=msbuild_project  -Dsonar.host.url=http://localhost:9000'*/

                    bat 'dotnet "C:\\Users\\shewine\\.dotnet\\tools\\.store\\dotnet-sonarscanner\\5.10.0\\dotnet-sonarscanner\\5.10.0\\tools\\net5.0\any\\SonarScanner.MSBuild.dll" begin  /d:sonar.login=${sonar-mvnwebaap} /k:"msbuild_project"'
                    bat 'dotnet build %WORKSPACE%\\ConsoleApp\\ConsoleApp.sln'
                    bat 'dotnet "C:\\Users\\shewine\\.dotnet\\tools\\.store\\dotnet-sonarscanner\\5.10.0\\dotnet-sonarscanner\\5.10.0\\tools\\net5.0\\any\\SonarScanner.MSBuild.dll" end /d.sonar.login=${sonar-mvnwebaap} '
                }  
            }
        }   

        

        
    }
}
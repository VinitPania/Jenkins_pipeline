pipeline{
    agent any
    stages{

            stage('Clean Workspace'){
                steps{
                    cleanWs()
                }
            }
        

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
                    bat  'dotnet  msbuild %WORKSPACE%\\ConsoleApp\\ConsoleApp.sln /p:Configuration=Release'
                    //bat  'dotnet  build  %WORKSPACE%\\ConsoleApp\\ConsoleApp.sln /p:Configuration=Release' 
                
                    // script{
                    //     def currentStatus = currentBuild.getCurrentStage().getResult()
                    //         echo "Current stage status: ${currentStatus}"

                    //     //if(${currentStep.result} == 'success'){
                    //     //    echo "Building successfully"
                    //     //}else if(${currentStep.result} == 'unstable'){
                    //     //    echo "Building unstable"
                    //     //}else ($BUILD_STATUS == 'failure'){
                    //     //    echo "Building failure"
                    //     //}
                    // }
                }   
                post{
                    always{
                        echo "====++++always++++===="
                    }
                    success{
                        echo "====++++only when successful++++===="
                    }
                    failure{
                        echo "====++++only when failed++++===="
                    }
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

                        bat 'dotnet "C:\\Users\\shewine\\.dotnet\\tools\\.store\\dotnet-sonarscanner\\5.10.0\\dotnet-sonarscanner\\5.10.0\\tools\\net5.0\\any\\SonarScanner.MSBuild.dll" begin   /k:"msbuild_project"'
                        bat 'dotnet build %WORKSPACE%\\ConsoleApp\\ConsoleApp.sln'
                        bat 'dotnet test %WORKSPACE%\\ConsoleApp\\ConsoleApp.sln'
                        bat 'dotnet "C:\\Users\\shewine\\.dotnet\\tools\\.store\\dotnet-sonarscanner\\5.10.0\\dotnet-sonarscanner\\5.10.0\\tools\\net5.0\\any\\SonarScanner.MSBuild.dll" end  '
                    }  
                }
            } 

            stage('Deployment'){
                steps{
                    bat 'net stop "w3svc" '
                    bat 'E:\\devops\\virtualdir\\msbuild_project\\ConsoleApp\\bin\\Debug\\netcoreapp3.1\\ConsoleApp.exe'
                    bat 'net start "w3svc"'
                    //bat 'xcopy  %WORKSPACE%\\ConsoleApp      E:\\devops\\virtualdir\\msbuild_project  /y /s'  
                }
            } 
        }

        post{
            always{
                archiveArtifacts artifacts: '**/*.exe', followSymlinks: false
                archiveArtifacts artifacts: '**/*.dll', followSymlinks: false
                archiveArtifacts artifacts: '**/*.pdb', followSymlinks: false
                archiveArtifacts artifacts: '**/*.json', followSymlinks: false
            }

            success{
                mail  body: 'The pipeline executed successfully',  from: 'vinit.pania@cloverinfotech.com',  subject: 'Pipeline Success', to: 'vinit.pania@cloverinfotech.com'
            }
            failure{
                mail  body: 'The pipeline failed',  subject: 'Pipeline Failure', to: 'vinit.pania@cloverinfotech.com'
            }
            unstable{
                mail  body: 'The pipeline is unstable',  subject: 'Pipeline unstable', to: 'vinit.pania@cloverinfotech.com'
            }
            changed{
                mail  body: 'The pipeline has changed ', subject: 'Pipeline changed', to: 'vinit.pania@cloverinfotech.com'
            }
        }
}
pipeline{
    agent any 
    environment{
        msbuild = "C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\MSBuild\\Current\\Bin\\MSBuild.exe"
        sonar-scanner = "C:\\Users\\shewine\\.dotnet\\tools\\.store\\dotnet-sonarscanner\\5.10.0\\dotnet-sonarscanner\\5.10.0\\tools\\net5.0\\any\\SonarScanner.MSBuild.dll"
    }
    stages{
        // stage('Cleanup Workspace'){
        //     steps{
        //         echo "====++Cleaning Workspace++++===="
        //         cleanWs()
        //     }
        // }

        stage("workspace"){
            steps{
            echo "'Running on' ${env.BUILD_ID} 'on URL' ${env.JENKINS_URL} 'on workspace' ${WORKSPACE}"
            }
        }

        stage("DotNet Version"){
            steps{
                echo "====++++executing DotNet Version++++===="
                bat 'dotnet --version'
            }
        }


        stage("checkout"){
            steps{
                echo "========Checking Out========"
            }
        }


        stage("Backup"){
            steps{
                echo "====++++executing Backup++++===="
                bat  'xcopy "E:\\devops\\clover-code\\19th JAN Application"   "E:\\devops\\backup\\QMS-Backup"  /v /s /y'
            }
        }

        // stage('Restore'){
        //     steps{
        //         echo "====++++Restore++++===="
        //         bat  'dotnet build  "E:\\devops\\clover-code\\19th JAN Application\\QMS-project final-18 August\\clover.qms.web\\clover.qms.web.sln"'
        //     }
        // }
        


        stage("Building"){
            steps{
                echo "====++++executing Building++++===="
                //bat 'dotnet build  "E:\\devops\\clover-code\\19th JAN Application\\QMS-project final-18 August\\clover.qms.web\\clover.qms.web.sln"  /t:Rebuild /p:Configuration=Release'
                bat '${env.msbuild} "E:\\devops\\clover-code\\19th JAN Application\\QMS-project final-18 August\\clover.qms.web\\clover.qms.web.sln"'
                //bat '"C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\MSBuild\\Current\\Bin\\MSBuild.exe" "E:\\devops\\clover-code\\19th JAN Application\\QMS-project final-18 August\\clover.qms.web\\clover.qms.web.sln"'
            }
            post{
                
                success{
                    echo "====++++Building executed successfully++++===="
                }
                failure{
                    echo "====++++Building execution failed++++===="
                }
        
            }
        }

        
        // stage("Testing"){
        //     steps{
        //         echo "====++++executing Testing++++===="
        //         bat 'dotnet test "E:\\devops\\clover-code\\19th JAN Application\\QMS-project final-18 August\\clover.qms.web\\clover.qms.web.sln"'
        //     }
        // }


        stage("CodeQuality"){
            steps{
                echo "====++++executing CodeQuality++++===="
                withSonarQubeEnv('SQ1'){
                    bat '${sonar-scanner}  begin   /k:"QMS_project" '
                    //bat 'dotnet "C:\\Users\\shewine\\.dotnet\\tools\\.store\\dotnet-sonarscanner\\5.10.0\\dotnet-sonarscanner\\5.10.0\\tools\\net5.0\\any\\SonarScanner.MSBuild.dll" begin   /k:"QMS_project"'
                    
                    
                    //bat 'dotnet build  "E:\\devops\\clover-code\\19th JAN Application\\QMS-project final-18 August\\clover.qms.web\\clover.qms.web.sln"  /p:Configuration=Release'
                    bat '${env.msbuild} "E:\\devops\\clover-code\\19th JAN Application\\QMS-project final-18 August\\clover.qms.web\\clover.qms.web.sln"'
                    
                    
                    //bat 'dotnet "C:\\Users\\shewine\\.dotnet\\tools\\.store\\dotnet-sonarscanner\\5.10.0\\dotnet-sonarscanner\\5.10.0\\tools\\net5.0\\any\\SonarScanner.MSBuild.dll" end'
                    bat '${sonar-scanner} end'
                }
            }
        }


        stage("Deploy"){
            steps{
                echo "====++++executing Deploy++++===="
                bat  'xcopy "E:\\devops\\clover-code\\19th JAN Application"   "E:\\devops\\virtualdir\\19th JAN Application"  /v /s /y'
            }
           
        }

    }


    post{
        always{
            echo "========Archive Artifacts========"
            archiveArtifacts artifacts: '**/*.exe', followSymlinks: false
            archiveArtifacts artifacts: '**/*.dll', followSymlinks: false
            archiveArtifacts artifacts: '**/*.pdb', followSymlinks: false
            archiveArtifacts artifacts: '**/*.json', followSymlinks: false
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
        unstable {
            echo "====++++unstable++++===="
        }
        changed{
            echo "====++++changed++++===="
        }
    }
}
pipeline{
    agent any 
    stages{
        stage('Cleanup Workspace'){
            steps{
                echo "====++Cleaning Workspace++++===="
                cleanWs()
            }
        }

        stage("workspace"){
            steps{
            echo "'Running on' ${env.BUILD_ID} 'on URL' ${env.JENKINS_URL} 'on workspace' ${WORKSPACE}"
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
                bat  'xcopy "E:\\devops\\jenkins_home\\.jenkins\\workspace\\"19th JAN Application""   "E:\\devops\\backup\\QMS-Backup"  /v /s /y'
            }
        }


        stage("DotNet Version"){
            steps{
                echo "====++++executing DotNet Version++++===="
                bat 'dotnet --version'
            }
        }


        stage("Building"){
            steps{
                echo "====++++executing Building++++===="
                bat 'dotnet build'
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

        
        stage("Testing"){
            steps{
                echo "====++++executing Testing++++===="
                bat 'dotnet test'
            }
        }


        stage("CodeQuality"){
            steps{
                echo "====++++executing CodeQuality++++===="
                withSonarQubeEnv('SQ1'){

                }
            }
        }


        stage("Deploy"){
            steps{
                echo "====++++executing Deploy++++===="
                bat  'xcopy'
            }
           
        }

    }


    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
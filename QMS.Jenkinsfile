pipeline{
    agent any 
    stages{
        stage(checkout){
            steps{
                echo "========Checking Out========"
            }
        }

        stage("Backup"){
            steps{
                echo "====++++executing Backup++++===="
                bat  'xcopy'
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
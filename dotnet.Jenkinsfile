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
                bat  "xcopy E:\\devops\\jenkins_home\\.jenkins\\workspace\\msbuild_project  E:\\devops\\backup\\msbuild_backup  /v /s "
            }
        }

        
    }
}
pipeline{
    agent any 

    stages{
        stage('checkout'){
             steps{
                echo "checking out..."
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/VinitPania/msbuild_project_deploy.git']])
            }
        }

        stage('build'){
            steps{

            }
        }

        stage('test'){
            steps{

            }
        }

        stage('sonarQube'){
            withSonarQubeEnv('SQ1'){
                def sonarRunner = tool name: 'SonarScannerMSBuild', type: 'hudson.plugins.sonar.MsBuildSQRunnerInstallation'
            
            }
        }




    }
}
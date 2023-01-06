pipeline{
    agent any 
    tools {
        maven 'MAVEN_HOME'
    }

    stages{
        stage('checkout'){
            steps{
                bat 'checking out  from git hub'
            }
        }
    

        stage('Build'){
         steps{
                echo 'BUILDing the project'
                script{
                    //bat 'docker build -t mybuild/'project name'' //change the name nigga
                    bat 'doker --version'
                }
            }
        }

        stage('send it to the moon aka to docker hub'){
            steps{
                echo 'lunching it to the moon aka to docker hub'
                script{
                    //bat 
                    //bat 'docker push mybuild/mvnwebaap'
                    bat 'docker ps -a'
                }
            }
        }
    }
}    
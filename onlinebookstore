pipeline{
    agent any 
    tools{
        maven 'MAVEN_HOME'
    }

    

    stages{

        stage("Environment variables"){
            steps{
            echo "Running on ${env.BUILD_ID} on url ${env.JENKINS_URL} on workspace ${WORKSPACE} on java home${JAVA_HOME}"
            }        
        }



        stage("checkout"){
            steps{
                checkout scmGit(branches: [[name: '*/J2EE']], extensions: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/VinitPania/onlinebookstore-1.git']])
                //checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/VinitPania/onlinebookstore-1.git']])
                //checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/VinitPania/onlinebookstore-1.git']])
            }
        }


        stage('build'){
            steps{
                bat 'mvn clean package'
            }
        }

        stage('Code Quality'){
            steps{
                withSonarQubeEnv('SQ1'){
                    bat 'mvn clean verify sonar:sonar -Dsonar.projectKey=onlinebookstore -Dsonar.host.url=http://localhost:9000'
                }
            }
        }

        stage('test'){
            steps{
                bat 'mvn test'
            }
        }

        

        stage('Backup'){
            steps{
                bat 'xcopy E:\\devops\\windows_node2\\workspace\\onlinebookstore*.* E:\\devops\\backup\\onlinebookstore /c /v/s/e /y'
            }
        }

        stage('html-report'){
            steps{
                bat 'mvn surefire-report:report'
            }
        }

        stage('applying-css'){
            steps{
                bat 'mvn site -DgenerateReports=faliure'
            }
        }

        stage('Deploying to tomcat'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'a3280777-1f55-4dc9-8bd4-d1ca024f41ef', path: '', url: 'http://localhost:8041/')], contextPath: 'onlinebookstore', war: '**/*.war'
            }
        }
    }

    post{
        always{
            archiveArtifacts artifacts: '**/*.jar'
            junit(allowEmptyResults:true, testResults: "**/target/surefire-reports/*.xml")
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site', reportFiles: 'surefire-report.html', reportName: 'SummaryHTMLReport', reportTitles: ''])
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
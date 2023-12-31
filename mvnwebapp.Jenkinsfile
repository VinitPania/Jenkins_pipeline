pipeline{
    agent {label "windows-node"}

    tools{
        maven "MAVEN_HOME"
    }

    stages{

        stage("environment-variable"){
            steps{
                echo "Running on ${env.BUILD_ID} on Url :${env.JENKINS_URL} on Workspace :${WORKSPACE} on Java_home ${env.JAVA_HOME}"
            }
        }

        stage("checkout"){
            steps{
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/VinitPania/mvnwebapp.git']])
            }
        }

        stage("build"){
            steps{
                echo 'Building'
                bat 'mvn -Dmaven.test.failure.ignore=true clean package'
            }
        }
        stage("code-quality"){
            steps{
                withSonarQubeEnv('SQ1'){
                echo 'Code Quality'
                bat  'mvn clean verify sonar:sonar  -Dsonar.projectKey=mvnwebapp  -Dsonar.host.url=http://localhost:9000 '
                }
            }
        }

        stage('backup'){
            steps{
                echo 'Taking backup' //for haveing space in our folder we have to add double quotes on the folder name eg:- E:\\devops\\"windows node2"\\workspace\\mvnwebapp
                bat '(xcopy "E:\\devops\\windows_node2\\workspace\\mvnwebapp"*.* "E:\\devops\\backup\\mvnwebapp" /c /v /s /e /y)'
            }
        }

        stage("testing"){
            steps{
                echo 'Testing'
                bat 'mvn  test -P coverage' //or bat 'mvn test '
            }
        }

        stage("html-report"){
            steps{
                echo 'HTML-Report'
                //bat 'mvn surefire-report:report'
                bat 'mvn surefire-report:report'
            }
        }

        stage("apply-css"){
            steps{
                echo 'Apply css-to report'
                bat 'mvn site -DgenerateReports=faliure'
            }
        }

        stage('Deploying to tomcat'){
            steps{
                echo 'Deploy to tomcat'
                //deploy adapters: [tomcat8(credentialsId: 'a3280777-1f55-4dc9-8bd4-d1ca024f41ef', path: '', url: 'http://localhost:8041/')], contextPath: 'mvnwebapp', onFailure: false, war: '**/*.war'
                deploy adapters: [tomcat8(credentialsId: 'a3280777-1f55-4dc9-8bd4-d1ca024f41ef', path: '', url: 'http://localhost:8041/')], contextPath: 'mvnwebapp', war: '**/*.war'
            }
        }
    }



    environment{
        EMAIL_From = 'vinit.pania@cloverinfotech.com'
        }
    
    post{

        always{
        echo "${WORKSPACE}"
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
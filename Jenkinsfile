pipeline {
    agent any
         stages {
         stage('Sonarqube') {
           environment {
                scannerHome = tool 'sonarScanner'
                }
         steps {
            withSonarQubeEnv('sonar') {
                sh "${scannerHome}/bin/sonar-scanner"
            }
            timeout(time: 3, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
          }
         }
         
     stage('Deploy') { 
           steps {
             sh ''' #! /bin/bash 
            
              aws deploy create-deployment --application-name ChatApp --deployment-group-name CFChatApp --deployment-config-name CodeDeployDefault.AllAtOnce --github-location repository=SaundaleB1995/ChatApplication,commitId=${GIT_COMMIT}
             '''
            }
        }      
    }
    post { 
        always { 
            echo 'chat application stage is success'
        }
    }   
}

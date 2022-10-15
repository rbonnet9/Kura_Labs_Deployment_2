pipeline {
  agent any
   stages {
    stage ('Build') {
      steps {
        sh '''#!/bin/bash
        python3 -m venv test3
        source test3/bin/activate
        pip install pip --upgrade
        pip install -r requirements.txt
        export FLASK_APP=application
        flask run &
        '''
     }
     /* 
     post{
        success { 
          slackSend channel: 'jenkinsnotifications',
                    color: 'good',
                    message: "Successful Build Stage!"
        }
        failure  {
          slackSend channel: 'jenkinsnotifications',
                    color: 'warning',
                    message: "Build Stage Failed!"
        }
      }
      */
   }
    stage ('test') {
      steps {
        sh '''#!/bin/bash
        source test3/bin/activate
        py.test --verbose --junit-xml test-reports/results.xml
        ''' 
      }
    
      post{
        always {
          junit 'test-reports/results.xml'
        }
        /*
        success { 
          slackSend channel: 'jenkinsnotifications',
                    color: 'good',
                    message: "Successful Test Stage!"
        }
        failure  {
          slackSend channel: 'jenkinsnotifications',
                    color: 'warning',
                    message: "Test Stage Failed!"
        }
        */
      }
    } 
     stage ('Deploy') {
       steps {
         sh '/var/lib/jenkins/.local/bin/eb deploy Deployment2-main-dev'
      }
      /*
      post{
       success { 
         slackSend channel: 'jenkinsnotifications',
                    color: 'good',
                    message: "Successful Deployment Stage!"
        }
        failure  {
          slackSend channel: 'jenkinsnotifications',
                    color: 'warning',
                    message: "Deployment Stage Failed!"
        }
        */
      }
    }
  }

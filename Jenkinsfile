pipeline {
  agent any
    
  tools {nodejs "NodeJS"}

  environment {
        EMAIL_TO = 'kenmoses033@gmail.com'
    }
    
  stages {
        
    stage('Git') {
      steps {
        git 'https://github.com/kenmoses1/Master.git'
      }
    }
     
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }  
    
            
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('Deploy') {
      steps {
        sh 'curl "https://api.render.com/deploy/srv-cgi2c882qv2772h2itag?key=0u5GYoaRWFU" --ssl-no-revoke'
      }
    }
  }

  post {
        always{
            slackSend( channel: "#moses_ip1", token: "slack_webhook token", color: "good", message: "${custom_msg()}")
        }

        failure{
            slackSend( channel: "#moses_ip1", token: "slack_webhook token", color: "good", message: "${custom_failure_msg()}")
        }

        unstable {
            emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Unstable build in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        }
        changed {
            emailext body: 'Check console output at $BUILD_URL to view the results.', 
                    to: "${EMAIL_TO}", 
                    subject: 'Jenkins build is back to normal: $PROJECT_NAME - #$BUILD_NUMBER'
        }
    }
}

def custom_failure_msg()
{
  def JENKINS_URL= "localhost:8080"
  def JOB_NAME = env.JOB_NAME
  def BUILD_ID= env.BUILD_ID
  def JENKINS_LOG= " FAILED: Job [${env.JOB_NAME}] Logs path: ${JENKINS_URL}/job/${JOB_NAME}/${BUILD_ID}/consoleText"
  return JENKINS_LOG
}

def custom_msg()
{
  def APP_URL= "https://gallery-5ryz.onrender.com"
  def JOB_NAME = env.JOB_NAME
  def BUILD_ID= env.BUILD_ID
  def JENKINS_LOG= " SUCCESSFUL: Job [${env.JOB_NAME}] Build Number: ${BUILD_ID} Site URL: ${APP_URL}"
  return JENKINS_LOG
}
pipeline {
  agent any
  stages {
    stage('User') {
      parallel {
        stage('User') {
          steps {
            sh 'docker exec -t constant_bigquery_etl python3 -m src.bigquery_merge merge_users_line'
          }
        }
      }
    }
    stage('Constant') {
      parallel {
        stage('Reserves') {
          steps {
            sh 'docker exec -t constant_bigquery_etl python3 -m src.bigquery_merge merge_reserves'
          }
        }
      }
    }
    stage('Saving') {
      parallel {
        stage('Saving_Termdeposit') {
          steps {
            sh 'docker exec -t constant_bigquery_etl python3 -m src.bigquery_merge merge_saving_termdeposit'
          }
        }
      }
    }
  }
  post {
    always {
      echo 'This will always run'
    }

    success {
      echo 'This will run only if successful'
    }

    failure {
      mail(bcc: '', body: "<b>Autonomous Data Platform Jenkins job FAILED</b><br>\n<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL of build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: 'data-team@autonomous.nyc', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "${env.EMAIL_RECIPIENTS}")
    }

    unstable {
      echo 'This will run only if the run was marked as unstable'
    }

    changed {
      echo 'This will run only if the state of the Pipeline has changed'
      echo 'For example, if the Pipeline was previously failing but is now successful'
    }

  }
  triggers {
    cron('H * * * *')
  }
}

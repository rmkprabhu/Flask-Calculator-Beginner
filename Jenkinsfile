pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh 'echo "building the repo"'
          }
        }
      }
    }

    stage('Test') {
      steps {
        echo "This is test stage"
      }
    }

    stage('Deploy')
    {
      steps {
        echo "deploying the application"
      }
    }

  }
  post {
        always {
            echo 'The pipeline completed'
            junit allowEmptyResults: true, testResults:'**/test_reports/*.xml'
        }
        success {
            sh "ls"
            sh "pwd"
            sh "hostname"
            sh "netstat -tulpn | grep LISTEN"
            sh "sudo nohup python3 app.py > log.txt 2>&1 &"
            echo "Flask Application Up and running!!"
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early…')
        }
      }
}

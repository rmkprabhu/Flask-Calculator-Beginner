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
            sh "pwd"
            sh "hostname"
            echo "killing old python process"
            sh "sudo pkill -f python"
            sh "netstat -tulpn | grep LISTEN"
            sh "whoami"
            echo "initiating new python process"
            sh "sudo nohup python app.py > log.txt 2>&1 &"
            sh "netstat -tulpn | grep LISTEN"
            echo "Flask Application Up and running!!"
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early…')
        }
      }
}

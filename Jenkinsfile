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
        sh "docker build -t python-test ."
        sh "docker run -d -p "5000:5000" -i python-test:latest"
      }
    }

  }
  post {
        always {
            echo 'The pipeline completed'
            junit allowEmptyResults: true, testResults:'**/test_reports/*.xml'
        }
        success {
            
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early…')
        }
      }
}

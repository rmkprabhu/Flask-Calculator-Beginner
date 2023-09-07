pipeline {
  agent 
   {
    label 'sonarqube-netflix'
   }
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
node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
}
    stage('Test') {
      steps {
        echo "This is test stage -yes"
      }
    }

    stage('Deploy')
    {
      steps {
        echo "deploying the application"
        sh "sudo docker build -t python-testjenkins ."
        sh 'sudo docker run -d -p "5000:5000" -i python-testjenkins:latest'
      }
    }

  }
  post {
        always {
            echo 'The pipeline completed'
            junit allowEmptyResults: true, testResults:'**/test_reports/*.xml'
        }
        success {
            echo " Deplopyment successful"
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early…')
        }
      }
}

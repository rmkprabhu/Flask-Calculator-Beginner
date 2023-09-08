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
    stage('sonarqube') {
      environment {
        scannerHome = tool 'Sonar-scanner'
      }
      steps {
      
      withSonarQubeEnv('Sonar-scanner') {
        sh "${scannerHome}/bin/sonar-scanner"
        }
      }
    }

    stage('Test') {
      steps {
        echo "This is test stag - yes"
        sh 'python3 test_app.py'
        input(id: "Deploy Gate", message: "Deploy ${params.project_name}?", ok: 'Deploy')
      }
    }

    stage('Deploy')
    {
      steps {
        echo "deploying the application"
        sh "sudo docker stop $(docker ps -q)"
        sh "sudo docker rm $(docker ps -aq)"
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

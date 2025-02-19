pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    HEROKU_API_KEY = credentials('lukasjai-heroku-key')
  }
  parameters {
    string(name: 'APP_NAME', defaultValue: 'java-web-app-jenkins-09059', description: 'What is the Heroku app name?')
  }


  stages {
  stage('Check Docker Access') {
              steps {
                  bat '''
                       echo ===================================
                                      echo   CHECKING DOCKER ACCESS
                                      echo ===================================
                                      docker --version
                                      docker info
                  '''
              }
          }
    stage('Build') {
      steps {
        bat '''
              cd %WORKSPACE%
              dir
              docker build --no-cache -t Lukasjai/masterarbeit_springboot_test_jenkis .
            '''
      }
    }
    stage('Login') {
      steps {
        bat 'echo %HEROKU_API_KEY% | docker login --username=_ --password-stdin registry.heroku.com'
      }
    }
    stage('Push to Heroku registry') {
      steps {
        bat '''
          docker tag Lukasjai/masterarbeit_springboot_test_jenkis:latest registry.heroku.com/%APP_NAME%/web
          docker push registry.heroku.com/%APP_NAME%/web
        '''
      }
    }
    stage('Release the image') {
      steps {
        bat "heroku container:release web --app=%APP_NAME%"      }
    }
  }
  post {
    always {
      bat 'docker logout'
    }
  }
}

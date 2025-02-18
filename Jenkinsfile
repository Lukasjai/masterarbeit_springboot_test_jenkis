pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    HEROKU_API_KEY = credentials('Lukasjai-heroku-api-key')
  }
  parameters {
    string(name: 'APP_NAME', defaultValue: '', description: 'What is the Heroku app name?')
  }
  stages {
    stage('Build') {
      steps {
        bat 'docker build -t Lukasjai/Masterarbeit_Springboot_Test .'
      }
    }
    stage('Login') {
      steps {
        bat 'echo $HEROKU_API_KEY | docker login --username=_ --password-stdin registry.heroku.com'
      }
    }
    stage('Push to Heroku registry') {
      steps {
        bat '''
          docker tag Lukasjai/Masterarbeit_Springboot_Test:latest registry.heroku.com/$APP_NAME/web
          docker push registry.heroku.com/$APP_NAME/web
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
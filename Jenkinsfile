pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout([$class: 'GitSCM',
          branches: [[name: '*/main']],
          userRemoteConfigs: [[url: '']]
        ])
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }
    stage('Deploy') {
      steps {
        deployTomcat(
          url: 'http://<tomcat-user>:<tomcat-pass>@<tomcat-host>:<tomcat-port>/manager/text',
          path: '/<app-context>',
          war: './target/<app-name>.war'
        )
      }
    }
  }
}

def deployTomcat(Map config) {
  sh "curl -v --upload-file ${config.war} '${config.url}/deploy?path=${config.path}&update=true'"
}

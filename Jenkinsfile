pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
          stage('Initialize'){
        def dockerHome = tool 'myDocker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
          stage("Docker build") {
               steps {
                      sh "docker build -t leszko/calculator:latest4"
               }
          }
       }
    }

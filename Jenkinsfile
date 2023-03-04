pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
         
          stage("Docker build") {
               steps {
                    sh "docker build -t leszko/calculator:latest1"
               }
          }
       }
    }

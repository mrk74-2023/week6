pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
            stage("Static code analysis") {
               steps {
                    sh "./gradlew checkstyleMain"
               }
          }
     }
}

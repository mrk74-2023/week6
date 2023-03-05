
pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          containers:
          - name: gradle
            image: gradle:6.3-jdk14
            command:
            - cat
            tty: true
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock    
        '''
    }
  }
  stages {
    stage('Build-Gradle-Test') {
      steps {
        container('gradle') {
          sh 'chmod +x gradlew'
          sh './gradlew test'
        }
      }
    }  
    stage('Build-Gradle-Coverage') {
      steps {
        container('gradle') {
          sh './gradlew jacocoTestCoverageVerification'
          sh './gradlew jacocoTestReport'
        }
      }
    } 
    stage('Build-Gradle-CodeAnalysis') {
      steps {
        container('gradle') {
          sh './gradlew checkstyleMain'
        }
      }
    } 
    stage('Build-Gradle-package') {
      steps {
        container('gradle') {
          sh './gradlew build'
        }
      }
    } 
        catch (Exception E) {
                        echo 'Failure detected'
                    }
    stage('Build-Docker-Image') {
      steps {
        container('docker') {
          sh 'docker build -t leszko/calculator:latest -f Dockerfile .'
        }
      }
    }
    stage('Login-Into-Docker') {
      steps {
        container('docker') {
          sh 'docker login -u mudassirmukhtar -p dckr_pat_hHGN8SHKbeECqJuyZML2ppOTKPU'
      }
    }
    }
    stage('Push-Images-Docker-to-DockerHub') {
      steps {
        container('docker') {
          sh 'docker tag leszko/calculator:latest mudassirmukhtar/demoproject:latest' 
          sh 'docker push mudassirmukhtar/demoproject:latest'
      }
    }
    }
  }
  post {
      always {
        container('docker') {
          sh 'docker logout'
      }
      }
    }
}

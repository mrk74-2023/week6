
pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
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
          sh 'docker tag cal leszko/calculator:latest'
          sh 'docker push leszko/calculator:latest'
      }
    }
   }
  }
}

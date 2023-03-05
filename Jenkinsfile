podTemplate(containers: [
    containerTemplate(
        name: 'docker', image: 'docker:latest', command: 'sleep', args: '30d', podRetention: 'onFailure()'
        ),
    ]) {

    node(POD_LABEL) {
        stage('Run pipeline against a docker project') {
            // "container" Selects a container of the agent pod so that all shell steps are executed in that container.
            container('docker') {
                stage('Build a docker image') {
                    // from the git plugin
                    // https://www.jenkins.io/doc/pipeline/steps/git/
                    git 'https://github.com/mrk74-2023/week6.git'
                    sh '''
                    docker build -t leszko/calculator:latest
                    '''
                }
            }
        }
    }

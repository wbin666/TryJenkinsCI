node {
    checkout scm
    stage('checkout') {
        docker.image('maven:3.3.3').inside {
            sh 'mvn --version'
        }
    }
    stage('Build') {
        sh 'echo "Hello World"'
        sh '''
            echo "Multiline shell steps works too"
            ls -lah
        '''
    }
}

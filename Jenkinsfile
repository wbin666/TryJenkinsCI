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
    stage('Deploy') {
        retry(3) {
            // sh './flakey-deploy.sh'
            sh 'echo "---step 01--function retry(3) in Deploy stage "'
        }

        timeout(time: 3, unit: 'MINUTES') {
            // sh './health-check.sh'
            sh 'echo "--step 02---function timeout(3) in Deploy stage "'
        }
    }
    
    try {
        stage('Test') {
            sh 'echo "Fail!"; exit 1'
        }
        echo 'This will run only if successful'
    } catch (e) {
        echo 'This will run only if failed'

        // Since we're catching the exception in order to report on it,
        // we need to re-throw it, to ensure that the build is marked as failed
        throw e
    } finally {
        def currentResult = currentBuild.result ?: 'SUCCESS'
        if (currentResult == 'UNSTABLE') {
            echo 'This will run only if the run was marked as unstable'
        }

        def previousResult = currentBuild.previousBuild?.result
        if (previousResult != null && previousResult != currentResult) {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }

        echo 'This will always run'
    }
}

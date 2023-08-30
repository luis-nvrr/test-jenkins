node {
    def stageResults = [:]

    stage('Build') {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            echo "Running build steps..."
            sh 'false' // Simulating a failing build step
        }
        stageResults['Build'] = 'FAILURE'
    }

    stage('Test') {
        steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                echo "Running test steps..."
                // Simulating an unstable test step
                unstable('This test is unstable')
            }
        }
        stageResults['Test'] = 'UNSTABLE'
    }

    stage('Deploy') {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            echo "Running deployment steps..."
            // No failure here
        }
        stageResults['Deploy'] = 'FAILURE'
    }

    if (stageResults.containsValue('FAILURE')) {
        currentBuild.result = 'FAILURE'
    } else if (stageResults.containsValue('UNSTABLE')) {
        currentBuild.result = 'UNSTABLE'
    }
}

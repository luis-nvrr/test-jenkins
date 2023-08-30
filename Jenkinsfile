node {
    def stageResults = [:]

    stage('Build') {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            echo "Running build steps..."
            sh 'false' // Simulating a failing build step
        }
        stageResults['Build'] = currentBuild.resultIsBetterOrEqualTo('FAILURE') ? 'FAILURE' : 'SUCCESS'
    }

    stage('Test') {
        steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                echo "Running test steps..."
                // Simulating an unstable test step
                unstable('This test is unstable')
            }
        }
        stageResults['Test'] = currentBuild.resultIsBetterOrEqualTo('FAILURE') ? 'FAILURE' : 'UNSTABLE'
    }

    stage('Deploy') {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            echo "Running deployment steps..."
            // No failure here
        }
        stageResults['Deploy'] = currentBuild.resultIsBetterOrEqualTo('FAILURE') ? 'FAILURE' : 'SUCCESS'
    }

    if (stageResults.containsValue('UNSTABLE')) {
        currentBuild.result = 'UNSTABLE'
    } else if (stageResults.containsValue('FAILURE')) {
        currentBuild.result = 'FAILURE'
    }
}

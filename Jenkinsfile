node {
    def stageResults = [:]

    stage('Build') {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            echo "Running build steps..."
            bat 'exit 1' // Simulating a failing build step on Windows
        }
        stageResults['Build'] = 'FAILURE'
    }

    stage('Test') {
        catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
            echo "Running test steps..."
            // Simulating an unstable test step
            unstable('This test is unstable')
             stageResults['Test'] = 'UNSTABLE'
        }
    
    }

    stage('Deploy') {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            echo "Running deployment steps..."
            bat 'echo Deployment steps on Windows' // Simulating deployment on Windows
            // No failure here
        }
        stageResults['Deploy'] = 'FAILURE'
    }

    if (stageResults.containsValue('UNSTABLE')) {
        currentBuild.result = 'UNSTABLE'
    } else if (stageResults.containsValue('FAILURE')) {
        currentBuild.result = 'FAILURE'
    }
}

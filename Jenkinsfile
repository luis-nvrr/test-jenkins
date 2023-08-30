node {
    stageResults = [:]
    runStageSavingResult  = {stageName, scriptBlock, stageFailResult -> 
        stage(stageName) {
            try {
                echo "Running ${stageName} steps..."
                scriptBlock()
            } catch (Exception e) {
                stageResults[stageName] = stageFailResult
                catchError(buildResult: 'SUCCESS', stageResult: stageFailResult) {
                    bat 'exit 1' // Simulating a failing build step on Windows
                }
            }
        }
    }

    runStageSavingResult('Build', {
        bat 'exit 1' // Simulating a failing build step on Windows
    }, 'FAILURE')

    runStageSavingResult('Test', {
         unstable('This test is unstable')
    }, 'UNSTABLE')

    runStageSavingResult('Deploy', {
        bat 'echo Deployment steps on Windows' // Simulating deployment on Windows
    }, 'FAILURE')


    if (stageResults.containsValue('UNSTABLE')) {
        currentBuild.result = 'UNSTABLE'
    } else if (stageResults.containsValue('FAILURE')) {
        currentBuild.result = 'FAILURE'
    }
}

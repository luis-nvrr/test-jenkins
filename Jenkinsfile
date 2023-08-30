node {
    stageResults = [:]
    runStageSavingResult  = {stageName, scriptBlock, stageFailResult, stageResults -> 
        stage(stageName) {
            try {
                echo "Running ${stageName} steps..."
                scriptBlock()
            } catch (error) {
                stageResults[stageName] = stageFailResult
                catchError(buildResult: 'SUCCESS', stageResult: stageFailResult) {
                    echo "Error: ${error}"
                    bat 'exit 1' // Simulating a failing build step on Windows
                }
            }
        }
    }

    runStageSavingResult('Build', {
        bat 'exit 1' // Simulating a failing build step on Windows
    }, 'FAILURE', stageResults)

    runStageSavingResult('Test', {
         bat 'exit 1' // Simulating a failing build step on Windows
    }, 'UNSTABLE', stageResults)

    runStageSavingResult('Deploy', {
        bat 'echo Deployment steps on Windows' // Simulating deployment on Windows
    }, 'FAILURE', stageResults)

    echo "Checking stageResults after all stages..."
        stageResults.each { stageName, stageResult ->
            echo "${stageName}: ${stageResult}"
        }
    
    if (stageResults.containsValue('UNSTABLE')) {
        currentBuild.result = 'UNSTABLE'
    } else if (stageResults.containsValue('FAILURE')) {
        currentBuild.result = 'FAILURE'
    }
}

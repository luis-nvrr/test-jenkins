
node {
    stageResults = [:]
     forceError = { error ->
     echo "Error: ${error}"
     if (isUnix()) {
         sh 'false' // Simulating a failing build step on Unix
    } else {
        bat 'exit 1' // Simulating a failing build step on Windows
    }
}
    runStageSavingResult  = {stageName, scriptBlock, stageFailResult, stageResults -> 
        stage(stageName) {
            try {
                echo "Running ${stageName} steps..."
                scriptBlock()
            } catch (error) {
                stageResults[stageName] = stageFailResult
                catchError(buildResult: 'SUCCESS', stageResult: stageFailResult) {
                    forceError(error)
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

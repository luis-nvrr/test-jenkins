node {
    stageResults = [:]
    
    stage('Build') {
        try {
          echo "Running build steps..."
            bat 'exit 1' // Simulating a failing build step on Windows
        } catch (Exception e) {
            stageResults['Test'] = 'FAILURE'
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                bat 'exit 1' // Simulating a failing build step on Windows
            }
        }
    }

    stage('Test') {
        try {
            echo "Running test steps..."
            // Simulating an unstable test step
            unstable('This test is unstable')
        } catch (Exception e) {
            stageResults['Test'] = 'UNSTABLE'
            catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                bat 'exit 1' // Simulating a failing build step on Windows
            }
        }
    }

    stage('Deploy') {
         try {
            echo "Running deployment steps..."
            bat 'echo Deployment steps on Windows' // Simulating deployment on Windows
            // No failure here
        } catch (Exception e) {
            stageResults['Deploy'] = 'FAILURE'
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                bat 'exit 1' // Simulating a failing build step on Windows
            }
        }
    }

    if (stageResults.containsValue('UNSTABLE')) {
        currentBuild.result = 'UNSTABLE'
    } else if (stageResults.containsValue('FAILURE')) {
        currentBuild.result = 'FAILURE'
    }
}

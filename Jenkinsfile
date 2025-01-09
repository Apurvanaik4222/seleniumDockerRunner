pipeline {
    agent any
    parameters {
        choice(
            name: 'BROWSER',
            choices: ['chrome', 'firefox'],
            description: 'Select the browser'
        )
    }

    stages {
        stage('Starting Grid') {
            steps {
                // Use the correct params syntax for passing the BROWSER parameter
                bat "docker compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
            }
        }
        stage('Running Tests') {
            steps {
                bat "docker compose -f testsuites.yaml up --pull=always"
            }
        }
    }

    post {
        always {
            // Ensure grid and tests are cleaned up after execution
            bat "docker compose -f grid.yaml down"
            bat "docker compose -f testsuites.yaml down"

            // Archive the test result
            archiveArtifacts artifacts: 'Results/em

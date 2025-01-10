pipeline {
    agent any

    parameters {
        choice(
            name: 'BROWSER',
            choices: ['chrome', 'firefox'],
            description: 'Select the browser to run tests on'
        )
    }

    stages {
        stage('Starting Grid') {
            steps {
                script {
                    // Dynamically scale the grid service for the selected browser
                    bat "docker compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
                }
            }
        }

        stage('Running Tests') {
            steps {
                script {
                    // Run test suites using Docker Compose
                    bat "docker compose -f testsuites.yaml up --pull=always"
                }
            }
        }
    }

    post {
        always {
            script {
                // Cleanup grid and test suite services
                bat "docker compose -f grid.yaml down || echo 'Grid already down'"
                bat "docker compose -f testsuites.yaml down || echo 'Test suites already down'"

                // Archive test results
                archiveArtifacts artifacts: 'Results/emailable-report.html', followSymlinks: false
            }
        }
    }
}

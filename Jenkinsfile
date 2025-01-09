pipeline{

    agent any
    {
    parameter{

    choices:choice ['chrome','firefox'], description: 'Select the browser', name: 'BROWSER'
    }
    }

        stages{

            stage('Starting Grid'){

                steps{
                    bat "docker compose -f grid.yaml up --scale ${param.BROWSER}=2 -d"
                }
            }
            stage('Running Tests'){

                steps{
                   bat "docker compose -f testsuites.yaml up --pull=always"
                }
            }
        }

        post{
            always{
                bat "docker compose -f grid.yaml down"
                bat "docker compose -f testsuites.yaml down"
                archiveArtifacts artifacts: 'Results/emailable-report.html', followSymlinks: false

            }
        }
    }

pipeline{

    agent any

    parameters{

        choice choices: ['chrome','firefox'],description: 'Select Browser for test',name:'BROWSER'
    }

        stages{

            stage('Starting Grid'){

                steps{
                    bat "docker compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
                }
            }
            stage('Running Tests'){

                steps{
                   bat "docker compose -f testsuites.yaml up"
                    script{
                      if(fileExist('Results/testng-failed.xml'){
                      error('Tests Failed')
                             }
                           }
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

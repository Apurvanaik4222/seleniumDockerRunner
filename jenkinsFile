pipeline{

    agent any{

        stages{

            stage('Starting Grid'){

                step{
                    bat "docker compose -f grid.yaml up -d"
                }
            }
            stage('Running Tests'){

                step{
                   bat "docker compose -f testsuites.yaml up"
                }
            }
        }

        post{
            always{
                bat "docker compose -f grid.yaml down"
                bat "docker compose -f testsuites.yaml down"
            }
        }
    }
}
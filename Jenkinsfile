pipeline {
    agent any

    stages {
        stage('Building jars') {
         agent{
            docker{
             image 'maven:3.9.3-eclipse-temurin-17-focal'
             args '-u root -v /tmp/m2:/root/.m2'

            }
         }

            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Creating an Image') {
            steps {
                script{
                    app =docker.build('apurvanaik422/seldocker100')
                }

            }
        }

        stage('Pushing Image to DockerHub') {
            steps {
                script{
                    docker.withRegistry('','dockercred'){
                        docker.push(latest)

                    }

                }
            }
        }
    }
}

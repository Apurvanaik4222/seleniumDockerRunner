pipeline {
    agent any

    stages {
        stage('Building JARs') {
            agent {
                docker {
                    image 'maven:3.9.3-eclipse-temurin-17-focal'
                    args '-u root -v /tmp/m2:/root/.m2'
                }
            }
            steps {
                // Handle any path-related issues by ensuring the workspace path is compatible
                script {
                    // Transform workspace path to Docker-compatible format
                    def transformedPath = env.WORKSPACE.replace('\\', '/').replace('C:', '/c')
                    echo "Transformed Workspace Path: ${transformedPath}"
                    // Execute Maven build
                    sh "mvn clean package -DskipTests"
                }
            }
        }

        stage('Creating an Image') {
            steps {
                script {
                    // Build the Docker image
                    app = docker.build('apurvanaik422/seldocker100')
                }
            }
        }

        stage('Pushing Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', 'dockercred') {
                        // Push the image with the 'latest' tag
                        app.push('latest')
                    }
                }
            }
        }
    }
}

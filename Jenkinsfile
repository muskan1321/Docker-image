pipeline {
    agent any
    
    environment {
        TRIVY_REPORT_FILE = 'trivy-report.json'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/muskan1321/Docker-image.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t muskan1321/my-docker-image:1.0 ." // Update with correct Dockerfile path and image tag
                }
            }
        }

        stage('Scan Docker Image with Trivy') {
            steps {
                script {
                    // Run Trivy to scan the Docker image
                    sh "trivy --format json --output ${TRIVY_REPORT_FILE} muskan1321/my-docker-image:1.0" // Update with correct image tag

                    // Archive the Trivy report as a build artifact
                    archiveArtifacts artifacts: TRIVY_REPORT_FILE, onlyIfSuccessful: false
                }
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PWD', usernameVariable: 'USR')]) {
                    sh "docker login -u ${USR} -p ${PWD}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh "docker push muskan1321/my-docker-image:1.0" // Update with correct image tag
            }
        }
    }

    post {
        always {
            // Clean up Docker images after the build
            cleanWs()
        }
    }
}


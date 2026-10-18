
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'GANTI_USERNAME/praktikum-simple-app'
        REGISTRY = 'https://index.docker.io/v1/'
        REGISTRY_CREDENTIALS = 'dockerhub-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'echo Mulai build aplikasi Python Flask'
                bat 'python --version'
                bat 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry(
                        REGISTRY,
                        REGISTRY_CREDENTIALS
                    ) {
                        def image = docker.image(
                            "${IMAGE_NAME}:${env.BUILD_NUMBER}"
                        )

                        image.push()
                        image.push('latest')
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline berhasil! Docker image berhasil dibuat dan dikirim ke Docker Hub.'
        }

        failure {
            echo 'Pipeline gagal. Periksa Console Output Jenkins.'
        }

        always {
            echo 'Selesai build'
        }
    }
}

pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = "prvinsm21"
        APP_NAME = "gitops-arocd-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }
    stages {
        stage ('Cleanup Workspace') {
            steps {
                script{
                    cleanWs()
                }
            }
        }
        stage ('Checkout SCM') {
            steps {
                script{
                    git credentialsId: 'github2', 
                    url: 'https://github.com/prvinsm21/gitops_argocd_proj.git',
                    branch: 'master'
                }
                }
            }
        stage ('Build Docker image') {
            steps {
                script {
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage ('Push Docker image') {
            steps {
                script {
                    docker.withRegistry('',REGISTRY_CREDS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage ('Delete Docker images') {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage ('Update kubernetes deployment file') {
            steps {
                script {
                    sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}:*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yml

                    """
                }
            }
        } 
        }
    }

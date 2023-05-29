pipeline {
    agent any
    environment {
        DOCKERHUB_USERNAME = "prvinsm21"
        APP_NAME = "gitops-arocd-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CRED = 'docker-cred'
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
        }
    }

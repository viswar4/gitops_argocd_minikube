pipeline{

    agent any

    environment{
        DOCKERHUB_USERNAME = "viswar4"
        APP_NAME = "gitops-argo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        DOCKERHUB_CREDS = 'dockerhub'
    }

    stages{
        
        stage("Clean Workspace"){
            
            steps{
                script{
                    
                    cleanWs()
                }
            }
        }

        stage("Checkout SCM"){
            
            steps{

                script{
                    git 'https://github.com/viswar4/gitops_argocd_minikube.git'
                }
                
                }
            }
        stage("Build Docker Image"){
            
            steps{

                script{
                    
                    docker_image = docker.build "${IMAGE_NAME}"
                }
                
                }
            }
        stage("Push Docker Image to Dockerhub"){
            
            steps{

                script{
                    
                    docker.withRegistry('', DOCKERHUB_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
                
                }
            }
        stage("Delete docker images and logout"){
            
            steps{

                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                    sh "docker logout"
                    }
                }
                
                }
            }
        }
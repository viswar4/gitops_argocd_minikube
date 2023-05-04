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
        }
    }
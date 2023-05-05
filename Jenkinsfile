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
        stage("Update K8's deployment file with Jenkins build number"){
 
            steps{

                script{
                    sh """
                     cat deployment.yaml
                     sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                     cat deployment.yaml
                    """
                }
            }
        }

        stage("Push the changed deployment file to git"){

            steps{

                script {
                    sh """
                      git config --global user.name "Raghav"
                      git config --global user.email "raghav@gmail.com"
                      git add deployment.yaml
                      git commit -m "updated deployment file"
                      """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/viswar4/gitops_argocd_minikube.git master"

                }
            }
        }
            }
        }
}
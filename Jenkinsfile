pipeline {
    agent any

    tools {
        jdk 'Java17'
        maven "Maven3"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/SalindaGunarathna/Otto-rent-Argocd_Kubernetes'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                script {
                    def newTag = "23.${env.BUILD_NUMBER}"
                    sh """
                        echo "Updating deployment.yaml with new image tag..."
                        cat deployment.yaml
                        sed -i 's|salindadocker/otto-rent-backend:.*|salindadocker/otto-rent-backend:${newTag}|g' deployment.yaml
                        cat deployment.yaml
                    """
                }
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                script {
                    def newTag = "23.${env.BUILD_NUMBER}"
                    sh """
                        git config --global user.name "YourUsername"
                        git config --global user.email "your.email@example.com"
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest with new image tag: ${newTag}"
                    """
                }
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/SalindaGunarathna/Otto-rent-Argocd_Kubernetes main"
                }
            }
        }
    }
}

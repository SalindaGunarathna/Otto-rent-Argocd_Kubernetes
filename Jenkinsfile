pipeline {
    agent {
        label "jenkins-agent"
    }

    tools{
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
                sh """
                   echo "Updating deployment.yaml with new image tag..."
                   cat deployment.yaml
                   sed -i 's|salindadocker/otto-rent-backend:.*|salindadocker/otto-rent-backend:${IMAGE_TAG}|g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "YourUsername"
                   git config --global user.email "your.email@example.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest with new image tag: ${IMAGE_TAG}"
                """
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                  sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/SalindaGunarathna/Otto-rent-Argocd_Kubernetes main"
                }
            }
        }
    }
}

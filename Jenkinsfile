pipeline {
    agent any
    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
      }
    stages{
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout From SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/krishnamsg/gitops-complete-production-e2e-pipeline'
            }
        }
        stage('Update the Deployemnt Tags') {
            steps {
                sh """
                    cat guestbook-ui-deployment.yaml
                    sed -i 's/${APP_NAME}.*${IMAGE_TAG}/g' guestbook-ui-deployment.yaml
                    cat guestbook-ui-deployment.yaml
                """
            }
        }
        stage('Push the changed deployment to GIT') {
            steps {
                sh """
                    git config --globel user.name "krishnamsg"
                    git congig --globel user.email "krishnams.aws1@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/krishnamsg/gitops-complete-production-e2e-pipeline main"
                    
                }
            }
        }
    }
}
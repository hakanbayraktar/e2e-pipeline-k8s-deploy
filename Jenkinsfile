pipeline{
    agent{
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "e2e-pipeline"
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/hakanbayraktar/e2e-pipeline-k8s-deploy'
            }

        }

        stage("Update to Deployment Tags"){
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }

        }

        stage("Patch the chnaged deployment file to Git"){
            steps {
                sh """
                   git config --global user.name "hakanbayraktar"
                   git config --global user.email "test@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifes"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-pat',gitToolName:'Default')]) {
                    sh "git push https://github.com/hakanbayraktar/e2e-pipeline-k8s-deploy main"
                } 
            }

        }
    }
        
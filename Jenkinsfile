pipeline {
    agent any
    environment{
        APP_NAME = 'ls-gitops'
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USENAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'docker-hub'
    }
    
    stages {
        stage('Cleanup Workspace'){
            steps {
                script {
                    cleanWs()
                }
            }
        }

        stage('Checkout to main') {
            steps {
                git branch: 'main', url: 'https://github.com/octopus237/Gitops-project-app'
            }
        }
        
        stage('Updating Kubernetes deployment file'){
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE'){
                    withCredentials([usernamePassword(credentialsId: 'my-github', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
                        sh "git config user.email jobri237@gmail.com"
                        sh "git config user.name octopus237"
                        sh "cat manifests/deployment.yaml"
                        sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${DOCKERTAG}/g' manifests/deployment.yaml"
                        sh "cat manifests/deployment.yaml"
                        sh "git add manifests/*"
                        sh "git commit -m 'Deployment file updated by Jenkins: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USER}:${GIT_PASS}@github.com/${GIT_USER}/Gitops-project-app.git"
                    }
                 }
            }
        }
    }
}

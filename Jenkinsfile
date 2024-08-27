node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update Deployment YAML') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email lassoued.mohamed@esprit.tn"
                    sh "git config user.name Mohamed Lassoued"
                    
                    // Replace placeholders in deployment.yml
                    sh """
                    sed -i 's+DOCKER_IMAGE_TAG+${DOCKERTAG}+g' deployment.yml
                    sed -i 's+BUILD_NUMBER+${env.BUILD_NUMBER}+g' deployment.yml
                    """
                    
                    sh "cat deployment.yml"
                    sh "git add ."
                    sh "git commit -m 'Updated Docker image tag to ${DOCKERTAG} and added change-cause annotation'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/jenkins-gitops-k8s.git HEAD:main"
                }
            }
        }
    }
}

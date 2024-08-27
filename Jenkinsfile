node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email lassoued.mohamed@esprit.tn"
                    sh "git config user.name Mohamed Lassoued"
                    sh "cat deployment.yml"
                    
                    // Replace the image tag in deployment.yml
                    sh "sed -i 's+medlas/jenkins-flask:.*+medlas/jenkins-flask:${DOCKERTAG}+g' deployment.yml"
                    
                    // Add an annotation to force image pull
                    sh """
                    sed -i '/spec:/a \\n  annotations:\n    kubernetes.io/change-cause: "Build number ${env.BUILD_NUMBER}"' deployment.yml
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

node {
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
                    sh "sed -i 's+your-docker-repo/odoo:.*+your-docker-repo/odoo:${DOCKERTAG}+g' deployment.yml"
                    sh "cat deployment.yml"
                    sh "git add ."
                    sh "git commit -m 'Update Odoo deployment to ${DOCKERTAG}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/jenkins-gitops-k8s.git HEAD:main"
                }
            }
        }
    }
}

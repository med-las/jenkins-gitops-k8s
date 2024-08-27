node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    // Ensure git is configured correctly
                    sh "git config --global user.email 'lassoued.mohamed@esprit.tn'"
                    sh "git config --global user.name 'Mohamed Lassoued'"

                    // Display deployment.yml to confirm the changes
                    sh "cat Deployment.yml"

                    // Correct the sed command to use the proper repository and Docker image tag format
                    sh "sed -i 's+medlas/jenkins-flask:.*+medlas/jenkins-flask:${DOCKERTAG}+g' Deployment.yml"

                    // Display the modified deployment.yml to verify changes
                    sh "cat Deployment.yml"

                    // Stage, commit, and push changes back to the repository
                    sh "git add Deployment.yml"
                    sh "git commit -m 'Updated by Jenkins Job updatemanifest: ${env.BUILD_NUMBER}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/jenkins-gitops-k8s.git HEAD:main"
                }
            }
        }
    }

    stage('Deploy to Kubernetes') {
        script {
            // Use kubectl to apply the updated manifest file to your Kubernetes cluster
            withKubeConfig(credentialsId: 'kubeconfig') {
                sh 'kubectl apply -f Deployment.yml'
            }
        }
    }
}

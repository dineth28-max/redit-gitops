pipeline {
    agent any

    environment {
        APP_NAME = "dineth123412/reddit-clone-app"
        IMAGE_TAG = "1.0.0-${BUILD_NUMBER}"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/dineth28-max/redit-gitops'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh '''
                    echo "Updating deployment.yaml with new image tag..."
                    sed -i "s|image:.*|image: ${APP_NAME}:${IMAGE_TAG}|g" deployment.yaml
                    echo "Updated deployment.yaml content:"
                    cat deployment.yaml
                '''
            }
        }

        stage("Push the changed deployment file to GitHub") {
            steps {
                sh '''
                    git config --global user.name "dineth28-max"
                    git config --global user.email "sandakelumdineth120@gmail.com"

                    git add deployment.yaml
                    if git diff --cached --quiet; then
                        echo "No changes detected, skipping commit"
                    else
                        git commit -m "Updated Deployment Manifest"
                        git push https://github.com/dineth28-max/redit-gitops main
                    fi
                '''
            }
        }
    }
}

pipeline {
    agent { label "jenkins-agent" }
    environment {
        APP_Name = "devops-java"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/RanjeethAcharya/devops.jar-CD'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   echo "Before Update:"
                   cat deployment.yaml
                   sed -i 's|image: ranjeeth3302/devops-java:.*|image: ranjeeth3302/devops-java:${BUILD_NUMBER}|g' deployment.yaml
                   echo "After Update:"
                   cat deployment.yaml
                """
            }
        }

        stage("Push the Changed Deployment File to Git") {
            steps {
                sh """
                   git config --global user.name "ranjeeth3302"
                   git config --global user.email "ranjeetkumar933342@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/RanjeethAcharya/devops.jar-CD main"
                } // Closing brace for withCredentials
            } // Closing brace for steps
        } // Closing brace for stage
    } // Closing brace for stages
} // Closing brace for pipeline

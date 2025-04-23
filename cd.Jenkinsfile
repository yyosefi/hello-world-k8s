pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = "github-ssh" // Set in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: "${GIT_CREDENTIALS_ID}", url: 'git@github.com:yyosefi/hello-world-k8s.git'
            }
        }

        stage('Make Changes') {
            steps {
                script {
                    sh """
                      sed -i 's|image: yyosefi/my-nginx:.*|image: yyosefi/my-nginx:${BUILD_NUMBER}|g' k8s/deployment.yaml
                      git config user.email "jenkins@local"
                      git config user.name "jenkins"
                      git add k8s/deployment.yaml
                      git commit -m "ci: bump image to ${BUILD_NUMBER}"
                    """
                }
            }
        }


        stage('Push Changes') {
            steps {
                sshagent (credentials: [GIT_CREDENTIALS_ID]) {
                    sh 'git push origin HEAD:main'
                }
            }
        }
    }
}
pipeline {
    agent any
    environment {
        REGISTRY = 'registry.hub.docker.com'
        IMAGE     = 'yyosefi/my-nginx'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm: [
                    $class: 'GitSCM',
                    branches: [[name: 'main']], // Or your desired branch
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        credentialsId: 'github-ssh',
                        url: 'git@github.com:yyosefi/hello-world-k8s.git'
                    ]]
                ]
            }
        }
        stage('List Checked Out Files') {
            steps {
                sh 'ls -al' // Lists all files and directories with detailed information
                // Alternatively, for a simpler list:
                // sh 'ls -1'
            }
        }        
        stage('Build') {
            steps {
                script {
                    
                    docker.withRegistry("https://${REGISTRY}", 'dockerhub') {
                        docker.build("${IMAGE}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        // Add more stages as needed (Test, Deploy, etc.)
    }
}

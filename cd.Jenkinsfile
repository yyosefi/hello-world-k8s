pipeline {
  agent any
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

    stage('Update K8s Manifest') {
      steps {
        sh """
          sed -i 's|image: yyosefi/my-nginx:.*|image: yyosefi/my-nginx:${BUILD_NUMBER}|g' k8s/deployment.yaml
          git config user.email "jenkins@local"
          git config user.name "jenkins"
          git add k8s/deployment.yaml
          git commit -m "ci: bump image to ${BUILD_NUMBER}"
          git push origin main
        """
      }
    }
  }
}
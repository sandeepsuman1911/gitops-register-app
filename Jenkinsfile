pipeline {
  agent { label 'jenkins-agent' }
  environment {
    APP_NAME = "register-app-pipeline"
  }
  stages {
    stage("Cleanup Workspace"){
      steps {
        cleanWs()
      }
    }
    stage("Checkout from SCM"){
      steps {
        git branch: 'main', credentialsId: 'github', url: 'https://github.com/sandeepsuman1911/gitops-register-app' 
      }
    }
    stage("Update the deployment Tags"){
      steps {
        sh """
            cat deployment.yml
            sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
            cat deployment.yml
        """
      }
    }
    stage("Push the changed deployment file to git"){
      steps{
        sh """
            git config --global user.email "sandeepsuman1917@outlook.com"
            git config --global user.name "sandeepsuman1911"
            git add deployment.yml
            git commit -m "Updated Deplpoyment Menifest"
        """
        withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
          sh "git push https://github.com/sandeepsuman1911/gitops-register-app main"
        }
      }
    }
  }
}

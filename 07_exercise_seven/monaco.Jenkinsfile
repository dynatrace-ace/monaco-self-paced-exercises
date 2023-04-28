pipeline {
  agent {
    label 'monaco-runner'
  }
  environment {
    DT_API_TOKEN = credentials('DT_API_TOKEN')
    DT_TENANT_URL = credentials('DT_TENANT_URL')
  }
  stages {
    stage('[Validation] Config Deployment') {
      steps {
        container('monaco') {
          script{
            sh "monaco deploy manifest.yaml --dry-run"
          }
        }
      }
    }
    stage('Config Deployment') {
      steps {
        container('monaco') {
          script{
            sh "monaco deploy manifest.yaml"
          }
        }
      }
    }
  }
}

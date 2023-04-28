MANIFEST_FILE = "manifest.yaml"

pipeline {
  agent {
    label 'monaco-runner'
  }
  environment {
    DT_API_TOKEN = credentials('DT_API_TOKEN')
    DT_TENANT_URL = credentials('DT_TENANT_URL')
  }
  stages {
    stage('[Validation] Infrastructure Config Deployment') {
      steps {
        container('monaco') {
          script{
            sh "monaco deploy $MANIFEST_FILE --project infrastructure --dry-run"
          }
        }
      }
    }
    stage('Infrastructure Config Deployment') {
      steps {
        container('monaco') {
          script{
            sh "monaco deploy $MANIFEST_FILE --project infrastructure"
            sh "sleep 60"
          }
        }
      }
    }
    
    stage('[Validation] Apps Config Deployment') {
      steps {
        container('monaco') {
          script {
            sh "monaco deploy $MANIFEST_FILE --project apps --dry-run"
          }
        }
      }
    }
    stage('Apps Config Deployment') {
      steps {
        container('monaco') {
          script {
            sh "monaco deploy $MANIFEST_FILE --project apps"
          }
        }
      }
    }
  }
}

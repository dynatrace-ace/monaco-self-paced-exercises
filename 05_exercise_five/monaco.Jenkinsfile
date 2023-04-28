pipeline {
  agent {
    label 'monaco-runner'
  }
  environment {
    // Dynatrace credentials
    DT_API_TOKEN = credentials('DT_API_TOKEN')
    DT_TENANT_URL = credentials('DT_TENANT_URL')
    // Input parameters
    TEMPLATE_APP_NAME = "${params['App Name']}"
    TEMPLATE_APP_ENVIRONMENT = "${params['Environment']}"
    TEMPLATE_K8S_NAMESPACE = "${params['Kubernetes Namespace']}"
    TEMPLATE_APP_URL_PATTERN = "${params['Application URL pattern']}"
    TEMPLATE_HEALTH_CHECK_URL = "${params['Health check URL']}"
  }
  stages {
    stage('[Validation] Application Template Deployment') {
      steps {
        container('monaco') {
          script {
            sh "monaco deploy manifest.yaml --dry-run"
          }
        }
      }
    }
    stage('Application Template Deployment') {
      steps {
        container('monaco') {
          script {
            sh "monaco deploy manifest.yaml"
          }
        }
      }
    }
  }
}

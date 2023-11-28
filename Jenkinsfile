pipeline {
  agent any
  stages {
    stage('trivy check') {
      steps {
        sh 'trivy image --severity HIGH,CRITICAL sumergerepo/automated-reconciliation-module-test:alpha'
      }
    }

  }
}

pipeline {
  agent {label "inbound-agent-jdk11"}
  stages {
    // stage('git') {
    //   steps {
    //    git 'https://github.com/mostafa-abd-elrazik/devsecops-test.git'
    //   }
    // }
    stage('trivy check') {
      steps {
        sh 'trivy image --severity HIGH,CRITICAL sumergerepo/automated-reconciliation-module-test:alpha'
      }
    }

  }
}

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
        // sleep 360
        sh 'trivy --timeout 900s image --severity HIGH,CRITICAL sumergerepo/automated-reconciliation-module-test:alpha'
      }
    }

  }
}

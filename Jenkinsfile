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
        sh 'podman build -t docker.idp.system.sumerge.local/dummy-image .'
        sh 'trivy --timeout 900s image --severity HIGH,CRITICAL docker.idp.system.sumerge.local/dummy-image'
      }
    }

  }
}

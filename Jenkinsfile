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
        sh """cat <<EOF >>/etc/containers/registries.conf 
[[registry]] 
location = "docker.idp.system.sumerge.local" 
insecure = true  
"""
        sh 'cat /etc/containers/registries.conf'
        sh 'podman build --tls-verify=false -t docker.idp.system.sumerge.local/dummy-image .'
        // sh 'podman build --tls-verify=false -t docker.idp.system.sumerge.local/dummy-image .'
        // sh 'buildah build --tls-verify=false -t docker.idp.system.sumerge.local/dummy-image:0.1 .'
        sh 'trivy image --insecure  --timeout 900s  --severity HIGH,CRITICAL docker.idp.system.sumerge.local/dummy-image:0.1'
        // sh 'podman version'
        // sh 'buildah images'
        // sh 'buildah build --tls-verify=false -t docker.idp.system.sumerge.local/dummy-image .'
        // sh 'trivy --timeout 900s image --severity HIGH,CRITICAL docker.idp.system.sumerge.local/dummy-image'
      }
    }

  }
}

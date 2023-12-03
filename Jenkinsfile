pipeline {
  agent {label "inbound-agent-jdk11"}
    tools {
        maven 'maven'
    }

    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
        stage('Preparation') {
            steps {   
                git branch: 'preauth-api-token',
                url: 'http://10.0.0.129//gitops/devsecops2.git',
                credentialsId: 'nour-gitlab'
            }
        }

        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    echo "building devsecops2 Version 1.0.${BUILD_NUMBER}"
                '''
            }
        }

        stage('Scan check') {
              steps {
                script{
	            def scannerHome = tool 'sonar-scanner';
                withSonarQubeEnv("sonarqube") { 
		          sh "${scannerHome}/bin/sonar-scanner --version"
                }
              }
            }
        }

        stage('Scan') {
            steps {
                script {
                def scannerHome = tool 'sonar-scanner';
                    withSonarQubeEnv("sonarqube") {
                    sh "${tool("sonar-scanner")}/bin/sonar-scanner \
                    -Dsonar.projectKey=devsecops2 \
                    -Dsonar.sources=. \
                    -Dsonar.css.node=. \
                    -Dsonar.host.url=http://sonarqube.k8s.system.local \
                    -Dsonar.login=sqa_876495118ce969910596909c381be31900bdb02b"
                  }
                }
            }
        }

        stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./ --format HTML ', odcInstallation: 'DP-check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage ('Build') {
            steps {
                sh 'mvn clean package install -DskipTests' 
            }
            post {
                success {
                    sh 'echo success'
                }
            }
        }
        
        stage('Build Images') {
            steps {
                /* sh 'echo Image build preauthorized-integration-module'
                sh 'docker build --network host -t sumergerepo/preauthorized-integration-module:beta ./preauthorized-integration-module/' */
                
                sh 'echo Image build devsecops2'
                sh 'podman build --network host -t docker.idp.system.sumerge.local/devsecops2 ./ebc-mock-svc/'
                
                //sh 'echo Image build ebc-switch-connector'
                //sh 'docker build --network host -t sumergerepo/ebc-switch-connector:alpha ./ebc-switch-connector/'
                
                //sh 'echo Image build ipn-integration-connector'
                //sh 'docker build --network host -t sumergerepo/preauth-ipn-connector:alpha ./ipn-integration-platform-connector/'
            }
        }

        stage("TRIVY"){
            steps{
                sh " trivy image docker.idp.system.sumerge.local/devsecops2"
            }
        }

        stage('Push images') {
            steps {
                withCredentials([usernamePassword(credentialsId:"localrepo",usernameVariable:"USERNAME",passwordVariable:"PASSWORD")]) {
                    
                    sh "podman login -u ${USERNAME} -p ${PASSWORD} docker.idp.system.sumerge.local"

                    /* sh 'echo Push image preauthorized-integration-module'
                    sh 'docker push sumergerepo/preauthorized-integration-module:beta'
                    sh 'docker tag sumergerepo/preauthorized-integration-module:beta sumergerepo/preauthorized-integration-module:beta-1.0.${BUILD_NUMBER}'
                    sh 'docker push sumergerepo/preauthorized-integration-module:beta-1.0.${BUILD_NUMBER}' */
                    
                    sh 'echo Push image devsecops2'
                    sh 'podman push docker.idp.system.sumerge.local/devsecops2 --tls-verify=false'
                    /* sh 'docker tag sumergerepo/ebc-mock-svc:alpha sumergerepo/ebc-mock-svc:alpha-1.0.${BUILD_NUMBER}'
                    sh 'docker push sumergerepo/ebc-mock-svc:alpha-1.0.${BUILD_NUMBER}' */
                    
                    // sh 'echo Push image ebc-switch-connector'
                    // sh 'docker push sumergerepo/ebc-switch-connector:alpha'
                    
                    // sh 'echo Push image preauth-ipn-connector'
                    // sh 'docker push sumergerepo/preauth-ipn-connector:alpha'
                }    
            }
        }        
    }
}

  
  
  
//   stages {
//     // stage('git') {
//     //   steps {
//     //    git 'https://github.com/mostafa-abd-elrazik/devsecops-test.git'
//     //   }
//     // }
//     stage('trivy check') {
//       steps {
//         // sleep 360
//         sh """cat <<EOF >>/etc/containers/registries.conf 
// [[registry]] 
// location = "docker.idp.system.sumerge.local" 
// insecure = true  
// """
//         // sh 'cat /etc/containers/registries.conf'
//         sh 'podman build --tls-verify=false -t docker.idp.system.sumerge.local/dummy-image .'
//         // sh 'podman build --tls-verify=false -t docker.idp.system.sumerge.local/dummy-image .'
//         // sh 'buildah build --tls-verify=false -t docker.idp.system.sumerge.local/dummy-image:0.1 .'
//         sh 'trivy image --insecure  --timeout 7200s  --severity HIGH,CRITICAL http://docker.idp.system.sumerge.local/slave-trivy:latest'
//         // sh 'podman version'
//         // sh 'buildah images'
//         // sh 'buildah build --tls-verify=false -t docker.idp.system.sumerge.local/dummy-image .'
//         // sh 'trivy --timeout 900s image --severity HIGH,CRITICAL docker.idp.system.sumerge.local/dummy-image'
//       }
//     }

//   }
// }

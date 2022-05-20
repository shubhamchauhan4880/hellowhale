pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/shubhamchauhan4880/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("shubhamchauhan4880/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'Docker_hub_passs') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          withKubeConfig(caCertificate: '', clusterName: ' clutser-info', contextName: '', credentialsId: 'mykubeconfig', namespace: '', serverUrl: ' https://192.168.49.2:8443') {
                             sh 'kubectl apply -f hellowhale.yml'
}
        }
      }

  }

}
}

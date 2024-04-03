podTemplate(label: 'docker-build', 
  containers: [
    containerTemplate(
      name: 'git',
      image: 'alpine/git',
      command: 'cat',
      ttyEnabled: true
    ),
    containerTemplate(
      name: 'docker',
      image: 'docker',
      command: 'cat',
      ttyEnabled: true
    ),
  ],
  volumes: [ 
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'), 
  ]
) {
    node('docker-build') {
        def dockerHubCred = credentials('dockerhub-credential-id-pw')
        def dockerHubCred2 = 'dockerhub-credential-id-pw'
        def dockerHubCred3 = dockerhub-credential-id-pw
        def appImage
        
        stage('Checkout'){
            container('git'){
                checkout scm
                echo "DEBUGING Built dockerHubCred: ${dockerHubCred}"
            }
        }
        
        stage('Build'){
            container('docker'){
                script {
                    appImage = docker.build("dongyub/docker-hello-world-fork")
                    echo "DEBUGING Built Docker image: ${appImage}"
                }
            }
        }
        
        stage('Test'){
            container('docker'){
                script {
                    appImage.inside {
                        sh 'npm install'
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Push'){
            container('docker'){
                script {
                    docker.withRegistry('https://registry.hub.docker.com', dockerHubCred){
                        appImage.push("${env.BUILD_NUMBER}")
                        appImage.push("latest")
                    }
                }
            }
        }
    }
    
}


   pipeline {
       agent  {
        label 'nodejs'
       }
       environment {
           DOCKER_IMAGE = 'harbor.bahur:443/nodejs/nodejs-app:latest'

       }

       stages {
           stage('Checkout') {
               steps {
                   script {
                       checkout([$class: 'GitSCM',
                                 branches: [[name: '*/main']],
                                 userRemoteConfigs: [[url: 'https://github.com/ina-ene/nodejskub.git', credentialsId: 'github-token']]
                        ])
                   }
                }
           }
           stage('Build Docker Image') {
               steps {
                   script {
                       // Build the Docker image using the Dockerfile in the root of the repo
                       dockerImage = docker.build(DOCKER_IMAGE)
                   }
               }
           }

           stage('Push to Harbor') {
               steps {
            
                   script {
                       // Log in to Harbor and push the built image
                       docker.withRegistry('https://harbor.bahur:443', 'jenkins-harbor-robot') {
                           dockerImage.push()
                       }
                   }
               }
           }
       }
       
       post {
           always {
               cleanWs()
           }
       }
   }
   
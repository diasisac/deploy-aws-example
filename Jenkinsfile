pipeline {
  agent any

  tools {
    nodejs '18.16.0'
  }

  stages {

  stage('Run Test') {
    steps {
      script {
        sh 'npm install && npm run test'
      }
    }
  }
   stage('Build project image') {
    steps {
      script {
        dockerapp = docker.build("isacdias/deploy-aws-example-example:${env.BUILD_ID}", "-f ./Dockerfile ./")
      }
    }
   }

   stage('Remove container if exists') {
    steps {
      sh '''
            if [ $( docker ps -a -f name=deploy-aws-example | wc -l ) -eq 2 ]; then
              docker stop deploy-aws-example
              docker rm deploy-aws-example
            
            fi
      '''
    }
   }

   stage('Deploy image') {
    steps { 
      sh """
          docker run --name deploy-aws-example --env-file /etc/curso/env -p 3000:3000 -d -t danieleleaoe/deploy-aws-example:${env.BUILD_ID}
      """
    }
   }

  }
}
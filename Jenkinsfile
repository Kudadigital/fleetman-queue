pipeline {
   agent any

   environment {
      SERVICE_NAME = "fleetman-queue"
      ORGANIZATION_NAME = "kudadigital"
      YOUR_DOCKERHUB_USERNAME = "augeos" // (it doesn't matter if you don't have one)
     
      REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh '''echo No build required for Queue'''
         }
      }

      stage('Build and Push Image to Dockerhub') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
           sh 'docker login -u "${YOUR_DOCKERHUB_USERNAME}" -p Lazio205!'
           sh 'docker push "${REPOSITORY_TAG}"'
         }
      }

      stage('Deploy to Cluster') {
          steps {
            sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}

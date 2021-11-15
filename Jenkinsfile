pipeline {
   agent { label 'ubuntu' }
  tools {
    maven 'mvn'
  }
   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     MAVEN_HOME = tool name: 'mvn'
     SERVICE_NAME = "fleetman-api-gateway"
     REPOSITORY_TAG="dileep4444/poc-api-gateway:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git url: "https://github.com/subhashmaddali/fleetman-api-gateway.git"
         }
      }
      stage('Testing') {
         steps {


            sh '''mvn test'''
             
         }
      }

      stage('code quality tests using SonarQube and Build') {
         steps {


            sh '''mvn clean package sonar:sonar -Dsonar.host.url=https://sonarcloud.io/ -Dsonar.login=e6610749a4efee1b87f28d194695c352bdd7abad'''
             
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
           sh 'docker push ${REPOSITORY_TAG}'
         }
      }

      stage('Deploy to Cluster') {
          steps {
                    sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}

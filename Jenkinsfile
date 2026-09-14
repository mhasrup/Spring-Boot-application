pipeline {
    agent { label 'ubuntu' }
    environment {
            APP_NAME = 'order-service'
            IMAGE_TAG = "${BUILD_NUMBER}"
            REGISTRY = 'localhost:32000'
            IMAGE = "${REGISTRY}/${APP_NAME}:${IMAGE_TAG}"
            HELM_CHART = 'helm/order-service'
        }
    stages {
       stage('CheckOutGit') {
                   steps {
                       checkout([$class: 'GitSCM',
                           branches: [[name: 'main']],
                           doGenerateSubmoduleConfigurations: false,
                           extensions: [],
                           submoduleCfg: [],
                           userRemoteConfigs: [[credentialsId: 'laxman_github_token',
                               url: 'https://github.com/mhasrup/Spring-Boot-application.git']]])
                   }
               }

       stage('Build') {
                   steps {
                       echo 'Building Spring Boot application...'

                       dir('complete') {
                           sh '''
                               mvn clean package -DskipTests
                           '''
                       }
                   }
               }

       stage('Docker Build') {
                   steps {
                       echo "Building Docker image ${IMAGE}"

                       dir('complete') {
                           sh """
                               docker build -t ${IMAGE} .
                           """
                       }
                   }
               }
       stage('Push Image') {
                   steps {
                       echo "Pushing ${IMAGE} to MicroK8s registry..."

                       sh """
                           docker push ${IMAGE}
                       """
                   }
               }
       stage('Deployment') {
                   steps {
                       echo 'Deploying application using Helm...'

                       sh """
                           microk8s helm3 upgrade --install order-service ${HELM_CHART} \
                               --set image.repository=${REGISTRY}/${APP_NAME} \
                               --set image.tag=${IMAGE_TAG}
                       """
                   }
               }
       stage('Validation') {
                   steps {
                       echo 'Validating Kubernetes deployment...'

                       sh '''
                           microk8s kubectl rollout status deployment/order-service-order --timeout=120s

                           microk8s kubectl get pods

                           microk8s kubectl get svc order-service-order
                       '''
                   }
               }
       stage('API Validation') {
                   steps {
                       echo 'Validating Order API...'

                       sh '''
                           sleep 5

                           curl --fail http://192.168.64.2:30080/orders
                       '''
                   }
               }
       post {

               success {
                   echo 'CI/CD pipeline completed successfully!'
               }

               failure {
                   echo 'CI/CD pipeline failed.'
               }

               always {
                   echo "Build number: ${BUILD_NUMBER}"
               }
           }
    }
}
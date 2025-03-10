pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "amisha1407/amishafinalnew"
        BUILD_TIMESTAMP = new Date().format("yyyyMMdd-HHmmss", TimeZone.getTimeZone('UTC'))
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
        RANCHER_NAMESPACE = "default"
        RANCHER_DEPLOYMENT = "assignment2-deployment"
    }
    
    stages {
        stage("Checkout Code") {
            steps {
                script {
                    checkout scm
                }
            }
        }
        
        stage("Building Student Survey Application Image") {
            steps {
                script {
                    sh 'echo "Building Docker image..."'
                    sh "docker login -u amisha1407 -p 123ami456A@"

                    // Build Docker image with a timestamped tag
                    def customImage = docker.build("${DOCKER_IMAGE}:${BUILD_TIMESTAMP}", "--build-arg BUILD_ID=${BUILD_TIMESTAMP} .")
                }
            }
        }
        
        stage("Pushing Image to DockerHub") {
            steps {
                script {
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_TIMESTAMP}"
                }
            }
        }
        
        stage("Deploying to Rancher") {
            steps {
                script {
                    // sh "kubectl set image deployment/${RANCHER_DEPLOYMENT} container-0=${DOCKER_IMAGE}:${BUILD_TIMESTAMP} -n ${RANCHER_NAMESPACE}"
                    // sh 'kubectl set image deployment/assign2-deployment container-0=amisha1407/amishafinalnew:${BUILD_TIMESTAMP} -n default'
                    sh "kubectl set image deployment/assignent2-deployment assignment2=${DOCKER_IMAGE} --namespace=default"
                    // sh "kubectl set image deployment/assign2-deployment container-0=amisha1407/amishafinalnew:${BUILD_TIMESTAMP} -n default --insecure-skip-tls-verify"
                    sh "kubectl rollout restart deployment/${RANCHER_DEPLOYMENT} -n ${RANCHER_NAMESPACE}"
                }
            }
        }
    }
    
    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed. Check Logs."
        }
    }
}
pipeline{
    agent any

    stages{
        stage("git checkout"){
            steps{
                git branch: 'master', credentialsId: 'github_token', url: 'https://github.com/divyeshbhalekar/node-app-k8s-jenkins-pipeline'
                sh 'ls'
                echo " ========executing Github checkout/clone repo======== "
            }
        }
        // stage("Docker Build"){
        //     steps{
        //         echo "====++++executing docker build stage++++===="
        //         sh "docker build . -t demodockeracc/jenkins-node-k8s-app:${DOCKER_TAG}"
        //         echo "====++++ Docker Build stage completed===="
        //     }
        // }
        stage('Docker Build') {
            steps {
                echo "====++++Executing Docker Build stage++++===="
                script {
                    try {
                        sh 'docker --version' // Check Docker version
                        sh 'docker images' // List current Docker images
                        sh 'docker build . -t demodockeracc/jenkins-node-k8s-app:${DOCKER_TAG}'
                        echo "====++++ Docker Build stage completed===="
                    } catch (Exception e) {
                        echo "Docker build failed: ${e.message}"
                        currentBuild.result = 'FAILURE'
                        error "Stopping pipeline due to Docker build failure"
                    }
                }
            }
        }
        stage("Docker Login"){
            steps{
                echo "started docker login"
                withCredentials([string(credentialsId: 'DOCKER_HUB_CREDENTIALS', variable: 'DOCKER_HUB_CREDENTIALS')]) {
                sh "docker login -u demodockeracc -p ${DOCKER_HUB_CREDENTIALS}"
            }
         }
        
        }
        stage("Docker Push"){
            steps{
                sh "docker push demodockeracc/jenkins-node-k8s-app:${DOCKER_TAG} "
                echo "Image pushed to Docker hub"
            }
        }
    }   
}

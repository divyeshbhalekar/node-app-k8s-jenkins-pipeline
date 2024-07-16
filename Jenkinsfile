pipeline{
    agent any

    environment {
        dockerImage = ''
        registry = 'demodockeracc/node-app-k8s-jenkins-pipeline'
    }

    stages{
        stage("git checkout"){
            steps{
                git branch: 'master', credentialsId: 'github_token', url: 'https://github.com/divyeshbhalekar/node-app-k8s-jenkins-pipeline'
                sh 'ls'
                echo " ========executing Github checkout/clone repo======== "
            }
        }

        stage("Docker Build"){
            steps{
                echo "Divyesh started docker build"
                sh "docker build -t demodockeracc/jenkins-node-k8s-app:1  ."
                echo "====++++ Docker Build stage completed===="
            }
        }
        stage("Docker Login"){
            steps{
                echo "started docker login"
                withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_CREDENTIALS', passwordVariable: 'pass', usernameVariable: 'usr')]) {
                sh "echo ${pass} | docker login -u ${usr} --password-stdin"
            }
         }       
        }
        stage("Docker Push"){
            steps{
                sh "docker push demodockeracc/jenkins-node-k8s-app:1 "
                echo "Image pushed to Docker hub"
            }
        }
    }   
}

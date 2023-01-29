pipeline {
    environment {
        imagename = "yarlov21/practicejenkins"
        registryCredential = "jenkins_yared"
        dockerimage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git([url: 'https://github.com/yared-shewarade/practicejenkins.git', branch:'main', credentialsId: 'jenkins_yared'])
            }
        }
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build imagename
                }
            }
        }
        stage('Deploy Image'){
            steps{
                script {
                    docker.withRegistry('', registryCredential ) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $imagename:$BUILD_NUMBER"
                sh "docker rmi $imagename:latest"
            }
        }
    }
}

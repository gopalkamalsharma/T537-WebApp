pipeline{
    agent any
    environment{
        REPOSITORY = 'webapp'
        IMAGE_NAME = 'webapp'
        IMAGE_TAG = "${BUILD_NUMBER}"
        USERNAME = 'gopalsharma2504'
    }
    stages{
        stage("Checkout"){
            steps{
                checkout scm
            }
        }
        stage("build"){
            steps{
                sh'''
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }
        stage("DockerHub Login"){
            steps{
                sh '''
                echo "${DOCKER_PASSWORD}" | docker login -u "${USERNAME}" --password-stdin
                '''
            }
        }
        stage("Image Push"){
            steps{
                sh'''
                docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}
                docker push ${USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }
        stage("Deploy"){
            steps{
                sh'''
                docker run -d -p 80:80 ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }
    }
}
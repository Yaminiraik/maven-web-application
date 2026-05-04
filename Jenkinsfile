pipeline
{
    agent any

    tools
    {
        maven 'Maven_3.9.7'
    }

    environment
    {
        BUILD_NUM = "${BUILD_NUMBER}"
    }

    stages
    {
        stage('Git Checkout')
        {
            steps()
            {
                git branch: 'feature', url: 'https://github.com/Yaminiraik/maven-web-application.git'
            }
        }

        stage('Build Project')
        {
            steps()
            {
                sh 'mvn clean package'
            }
        }

        stage('Build  Docker Image')
        {
            steps()
            {
                sh 'docker build -t yaminiraik/dockerpipeline:${BUILD_NUM} .'
            }
        }

        stage('Docker login and image push to Dockerhub')
        {
            steps()
            {
                withCredentials([string(credentialsId: 'Docker_Hub_Password', variable: 'Docker_Hub_Password')]) 
                {
                    sh 'docker login -u yaminiraik -p ${Docker_Hub_Password}'
                }   
                sh 'docker push yaminiraik/dockerpipeline:${BUILD_NUM}'
            }
        }

        stage('Delete Docker Image locally after pushed to Dockerhub')
        {
            steps()
            {
                sh 'docker rmi -f yaminiraik/pipeline:${BUILD_NUM}'
            }
        }

        stage('Deploy Application to Docker Deployment Server')
        {
            steps()
            {
                sshagent(['DeploymentServer_SSH']) 
                {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.82.191.48 docker rm -f maven-web-application || true"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@54.82.191.48 docker run -d --name maven-web-application -p 8080:8080 yaminiraik/dockerpipeline:${BUILD_NUM}"
                }

            }
        }
    }    
}
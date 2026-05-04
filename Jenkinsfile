pipeline
{
    agent any

    tools
    {
        maven 'Maven_3.9.7'
    }

    environment
    {
        buildNumber = "${BUILD_NUMBER}"
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
                sh 'docker build -t yaminiraik/dockerpipeline:${buildNumber} .'
            }
        }

        stage('Docker login and image push to Dockerhub')
        {
            steps()
            {
                withCredentials([string(credentialsId: 'Docker_Hub_Password', variable: 'Docker_Hub_Password')]) 
                {
                    sh 'docker login -u yaminiraik -p ${Docker_Hub_password}'
                }   
                sh 'docker push yaminiraik/dockerpipeline:${buildNumber}'
            }
        }

        stage('Delete Docker Image locally after pushed to Dockerhub')
        {
            steps()
            {
                sh 'docker rmi -f yaminiraik/pipeline:${buildNumber}'
            }
        }
    }    
}
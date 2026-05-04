pipeline
{
    agent any
    tools
    {
        maven 'maven_3.9.7'
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
    }

    stages
    {
        stage('Build Project')
        {
            steps()
            {
                sh 'mvn clean package'
            }
        }
    }
}

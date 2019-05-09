pipeline {
    agent any
    stages {
        stage ('Build First JSP Project') 
        {
            steps 
            {
               bat  'mvn clean package'
            }
            post
            {
                success
                {
                    echo 'Now Archiving ....'
                    archiveArtifacts artifacts : '**/*.war'
                }
            }
        }

        stage ('Deploy successful build in Stage Environment')
        {
            steps
            {
                build job : 'Deploy-Stage-Pipeline'
            }
        }

        stage ('Deploy Successful build in UAT Environment')
        {
            steps
            {
                build job : 'Deploy-UAT-Pipeline' 
            }
            post
            {
                success
                {
                    echo 'Deployment on UAT Environment is Successful'
                }
                failure
                {
                    echo 'Deployement Failure on UAT Environment'
                }
            }
        }
    }
}

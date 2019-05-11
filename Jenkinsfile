pipeline {
    agent any
    stages {
        stage ('Build First JSP Project') 
        {
            steps 
            {
               bat  'mvn clean compile'			   
            }
            post
            {
                success
                {
                    echo 'compiled sucessfully ....'

                }
            }
        }
		
		stage ('Build Package')
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
		
		stage ('Execute Devops regression test suite and verify code quality')
		{
			steps
			{
				build job : 'DevOpsRegressionTests'
			}
			post
			{
				success
				{
					echo 'Regression test suite executed properly - we can gohead for UAT deployment'
				}
				failure
				{
					echo 'Regression test suite failed - Abort UAT deployment'
				}
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

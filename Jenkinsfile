pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
            	success {
            		echo 'Now Archiving...'
            		archiveArtifacts artifacts: '**/target/*.war'
            	}
            }
        }
        stage('Deploy to Staging'){
        	steps{
        		build job: 'MavenPro/Deploy'
        	}
        }
        stage('Deploy to Production'){
        	steps{
        		timeout(time:5, unit:'DAYS'){
        			input message:'Aprove PRODUCTION Deployment?'
        		}
        		build job:'deploy-to-prod'
        	}
        	post{
        		success{
        			echo 'Code Deployment to production.'
        		}
        		failure{
        			echo 'Deployment failed.'
        		}
        	}
        }
    }
}

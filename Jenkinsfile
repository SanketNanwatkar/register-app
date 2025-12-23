pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'Github2025', url: 'https://github.com/SanketNanwatkar/register-app'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }
		
	   stage("SonarQube Analysis") {
            steps {
                withSonarQubeEnv('sonarqube-server') {
                    sh '''
                      mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.11.0.3922:sonar \
                      -Dsonar.projectKey=register-app \
                      -Dsonar.projectName=register-app
                    '''
               		}
				}
	  		}
	    }
        stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }	
            }
			
   		 }
	}
}

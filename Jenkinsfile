pipeline {
	agent any
	tools {
        maven 'm3' 
    }
	stages {
    		stage('checkout') {
			input {
      	message "Should we continue?"
        ok "Yes, we should."
        parameters {
        	string(name: 'REPO', defaultValue: 'petclinic', description: 'Which repo you want to use?')
        }
			}
			steps {
	        		git "https://github.com/akmaharshi/${REPO}.git"
    			}
		}
    
    		stage('Build') 	{
			steps {
        			sh 'mvn package'
			}
    		}
    		stage('parallel stages') {
    		parallel {
    			stage('Archival') {
				    steps {
        				archiveArtifacts 'target/*.war'
				    }
    			}
		
			    stage('Test cases') {
				    steps {
        				junit 'target/surefire-reports/*.xml'
				    }
    		    }
		    } }
    
  	}
	post {
		always {
			notify('started')
		}
		failure {
			notify('err')
		}
		success {
			notify('success')
		}
	}
}

def notify(status){
    emailext (
    to: "devops.kphb@gmail.com",
    subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
    body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME}  [${env.BUILD_NUMBER}]</a></p>""",
    )
 }

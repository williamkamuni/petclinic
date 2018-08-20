pipeline {
	agent any
	environment {
		project_path = "spring-boot-samples/spring-boot-sample-atmosphere"
	}
	stages {
        	stage('compile, test and package') {
        		steps {
            			sh 'mvn clean package'
			}
        	}
        	stage('archival') {
        		steps {
            	 		archiveArtifacts 'target/*.jar'
            		}
        	}
        	stage('unit test') {
        		steps {
		            	junit 'target/surefire-reports/*.xml'
            		}
        	}
	}
	post { 
        	always { 
            	notify ('NOTIFY')
        	}
        	failure { 
        		notify ('FAIL')
        	}
        	success {
        		notify ('SUCCESS')
        	}
    	}
}
def notify(status){
    emailext (
      to: "anil.maharshi@gmail.com",
      subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} 	[${env.BUILD_NUMBER}]</a></p>""",
    	)
 }

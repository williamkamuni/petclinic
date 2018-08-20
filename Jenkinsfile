pipeline {
	agent any
	stages {
        	stage('compile, test and package') {
        		steps {
            			sh 'mvn clean package'
			}
        	}
            /*    stage('SonarQube analysis') { 
                        steps {
                                withSonarQubeEnv('Sonar') {
                                        steps {
                                        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar ' + 
                                        '-f pom.xml ' +
                                        '-Dsonar.projectKey=com.petclinic:all:master ' +
                                        '-Dsonar.login=admin ' +
                                        '-Dsonar.password=admin ' +
                                        '-Dsonar.language=java ' +
                                        '-Dsonar.sources=. ' +
                                        '-Dsonar.tests=. ' +
                            //            '-Dsonar.test.inclusions=**/*Test*/** ' +
                            //            '-Dsonar.exclusions=**/*Test*/**'
                                        }
                                }
                        }
                }
                stage("SonarQube Quality Gate") { 
                        steps {
                                timeout(time: 1, unit: 'HOURS') {  
                                        script {
                                                def qg = waitForQualityGate()
                                                if (qg.status != 'OK') {
                                                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                                                }
                                        }
                                }
                        }
                } */
        	stage('archival') {
        		steps {
            	 		archiveArtifacts 'target/*.?ar'
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

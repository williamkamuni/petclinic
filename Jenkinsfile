pipeline {
        agent any //{
               // docker {
               //         image 'maven:3-alpine'
              //          args '-v /root/.m2:/root/.m2'
              //  }
       // }
         tools {
        //         docker 'dockerlatest'
                   maven 'M3'
         }
        
        parameters {
                string (
                        defaultValue: '*',
                        description: '',
                        name : 'BRANCH_PATTERN')
                booleanParam (
                        defaultValue: false,
                        description: '',
                        name : 'PUBLISH')
                }
	stages {
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
                                stage('Publish') {
                                        when {
                                                expression {
                                                        //GIT_BRANCH = 'origin/' + sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                                                        //return GIT_BRANCH == 'origin/master' || params.FORCE_FULL_BUILD
                                                        params.PUBLISH
                                                }
                                        }
                                        input {
                                                message "Should we publish?"
                                                ok "Yes, we should."
                                        }
                                        steps {
                                                //nexusPublisher nexusInstanceId: 'nexus2', nexusRepositoryId: 'snapshots', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/petclinic.war']], mavenCoordinate: [artifactId: 'spring-petclinic', groupId: 'org.springframework.samples', packaging: 'war', version: '4.2.6-SNAPSHOT']]]
                                               // nexusArtifactUploader artifacts: [[artifactId: 'spring-petclinic', classifier: '', file: 'target/petclinic.war', type: 'war']], credentialsId: 'nexuscred', groupId: 'org.springframework.samples', nexusUrl: '192.168.100.12:8080/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'snapshots', version: '4.2.6-SNAPSHOT'

                                        }
                                }
		        } 
                }

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

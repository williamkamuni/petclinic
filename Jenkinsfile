pipeline {
	agent any
	tools {
        maven 'm3' 
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
                                        steps {
                                                //nexusPublisher nexusInstanceId: 'nexus2', nexusRepositoryId: 'snapshots', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/petclinic.war']], mavenCoordinate: [artifactId: 'spring-petclinic', groupId: 'org.springframework.samples', packaging: 'war', version: '4.2.6-SNAPSHOT']]]
                                                nexusArtifactUploader {
                                                        nexusVersion('nexus2')
                                                        protocol('http')
                                                        nexusUrl('192.168.100.12:8080/nexus')
                                                        groupId('org.springframework.samples')
                                                        version('4.2.6-SNAPSHOT')
                                                        repository('snapshots')
                                                        credentialsId('nexuscred')
                                                        artifact {
                                                                artifactId('spring-pipeline')
                                                                type('war')
                                                                classifier('debug')
                                                                file('pipeline.war')
                                                        }
                                                }
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

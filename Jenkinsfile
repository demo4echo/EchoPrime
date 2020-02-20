// We can omit this one as we marked the shared library to load implicitly
@Library('EchoSharedLibrary') _

// Load shared resources
def jenkinsSlavePodManifestResourceAsString = libraryResource 'jenkinsSlavePodManifest.yaml'

pipeline {
	agent {
		kubernetes {
			cloud pipelineCommon.resolveCloudNameByBranchName()
			label pipelineCommon.constructJenkinsSlavePodAgentLabel()
			defaultContainer pipelineCommon.K8S_AGENT_DEFAULT_CONTAINER
			yaml jenkinsSlavePodManifestResourceAsString
		}
	}
	options { 
		timestamps()
		
		buildDiscarder(logRotator(numToKeepStr: pipelineCommon.OPTIONS_BUILD_DISCARDER_LOG_ROTATOR_NUM_TO_KEEP_STR))
	}
	stages {
		stage('\u2776 Mark Service For Release \u2728') {
 			failFast true
			parallel {			
				stage ('\u2776.\u2776 Mark echobe For Release \u2728') {	
					steps {
						build (
							job: "echobe/${env.BRANCH_NAME}",
							parameters: [
								string (name: 'TARGET_JENKINSFILE_FILE_NAME', value: 'Jenkinsfile4Release'),
								string (name: 'TARGET_RECKON_SCOPE', value: 'major'),
								string (name: 'TARGET_RECKON_STAGE', value: 'final')
                		],
							wait: true
						)
					}
				}
				stage ('\u2776.\u2777 Mark echofe For Release \u2728') {	
					steps {
						build (
							job: "echofe/${env.BRANCH_NAME}",
							parameters: [
								string (name: 'TARGET_JENKINSFILE_FILE_NAME', value: 'Jenkinsfile4Release'),
								string (name: 'TARGET_RECKON_SCOPE', value: 'major'),
								string (name: 'TARGET_RECKON_STAGE', value: 'final')
                		],
							wait: true
						)
					}
				}
			}
		}
	}
	post {
		always {
			echo 'One way or another, I have finished'
		}
		success {
			echo 'I succeeeded!'
		}
		unstable {
			echo 'I am unstable :/'
		}
		failure {
			echo 'I failed :('
		}
		changed {
			echo 'Things were different before...'
		}
	}
}

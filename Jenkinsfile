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
	parameters {
		choice (
			name: 'TARGET_JENKINSFILE_FILE_NAME',
			choices: [
				pipelineCommon.PARAMS_TARGET_JENKINSFILE_FILE_NAME_OPTIONS[1],
				pipelineCommon.PARAMS_TARGET_JENKINSFILE_FILE_NAME_OPTIONS[0]
			],
			description: 'The desired Jenkinsfile to run'
		)
		choice (
			name: 'TARGET_RECKON_SCOPE',
			choices: [
				pipelineCommon.PARAMS_TARGET_RECKON_SCOPE_OPTIONS[3],
				pipelineCommon.PARAMS_TARGET_RECKON_SCOPE_OPTIONS[2],
				pipelineCommon.PARAMS_TARGET_RECKON_SCOPE_OPTIONS[1]
			],
			description: 'The desired reckon scope to use in the build'
		)
		choice (
			name: 'TARGET_RECKON_STAGE',
			choices: [
				pipelineCommon.PARAMS_TARGET_RECKON_STAGE_OPTIONS[3],
				pipelineCommon.PARAMS_TARGET_RECKON_STAGE_OPTIONS[2],
				pipelineCommon.PARAMS_TARGET_RECKON_STAGE_OPTIONS[1]
			],
			description: 'The desired reckon stage to use in the build'
		)
		validatingString (
			name: 'DESIGNATED_VERSION',
			defaultValue: pipelineCommon.PARAMS_DESIGNATED_VERSION_DEFAULT_VALUE,
			regex: pipelineCommon.PARAMS_DESIGNATED_VERSION_REG_EXP,
			failedValidationMessage: "Validation of designated version failed!",
			description: """
			The desiganted (desired) version to be used.
			Notes:
			------
			1. The input given must comply with 'Semantic Versioning 2.0.0' (https://semver.org) with regards to: <MAJOR>.<MINOR>.<PATCH>
			2. The version supplied must be higher than any existing version (tag) in the target repo(s)
			3. This will void the use of the 'TARGET_RECKON_SCOPE' and 'TARGET_RECKON_STAGE' parameters!
			"""
		)
	}	
	stages {
		stage('\u2776 Mark Service For Release \u2728') {
			when { 
				expression { 
					params.DESIGNATED_VERSION.trim().isEmpty() == true 
				} 
			}
 			failFast true
			parallel {			
				stage ('\u2776.\u2776 Mark echobe For Release \u2728') {	
					steps {
						build (
							job: "echobe/${env.BRANCH_NAME}",
							parameters: [
								string (name: 'TARGET_JENKINSFILE_FILE_NAME', value: "${params.TARGET_JENKINSFILE_FILE_NAME}"),
								string (name: 'TARGET_RECKON_SCOPE', value: "${params.TARGET_RECKON_SCOPE}"),
								string (name: 'TARGET_RECKON_STAGE', value: "${params.TARGET_RECKON_STAGE}")
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
								string (name: 'TARGET_JENKINSFILE_FILE_NAME', value: "${params.TARGET_JENKINSFILE_FILE_NAME}"),
								string (name: 'TARGET_RECKON_SCOPE', value: "${params.TARGET_RECKON_SCOPE}"),
								string (name: 'TARGET_RECKON_STAGE', value: "${params.TARGET_RECKON_STAGE}")
                		],
							wait: true
						)
					}
				}
			}
		}
		stage('\u2776 Mark Service For Designated Release \u2728') {
			when { 
				expression { 
					params.DESIGNATED_VERSION.trim().isEmpty() == false 
				} 
			}
 			failFast true
			parallel {			
				stage ('\u2776.\u2776 Mark echobe For Designated Release \u2728') {	
					steps {
						build (
							job: "echobe/${env.BRANCH_NAME}",
							parameters: [
								string (name: 'TARGET_JENKINSFILE_FILE_NAME', value: "${params.TARGET_JENKINSFILE_FILE_NAME}"),
								validatingString (name: 'DESIGNATED_VERSION', value: "${params.DESIGNATED_VERSION}"),
                		],
							wait: true
						)
					}
				}
				stage ('\u2776.\u2777 Mark echofe For Designated Release \u2728') {	
					steps {
						build (
							job: "echofe/${env.BRANCH_NAME}",
							parameters: [
								string (name: 'TARGET_JENKINSFILE_FILE_NAME', value: "${params.TARGET_JENKINSFILE_FILE_NAME}"),
								validatingString (name: 'DESIGNATED_VERSION', value: "${params.DESIGNATED_VERSION}"),
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

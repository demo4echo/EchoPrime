// We can omit this one as we marked the shared library to load implicitly
@Library('EchoSharedLibrary') _

// Load shared resources
def jenkinsSlavePodManifestResourceAsString = libraryResource 'jenkinsSlavePodManifest.yaml'

pipeline {
	agent {
		kubernetes {
			cloud pipelineCommon.resolveCloudNameByBranchName()
			label pipelineCommon.K8S_AGENT_LABEL
			defaultContainer pipelineCommon.K8S_AGENT_DEFAULT_CONTAINER
			yaml jenkinsSlavePodManifestResourceAsString
		}
	}
	options { 
		timestamps()
		
		buildDiscarder(logRotator(numToKeepStr: pipelineCommon.OPTIONS_BUILD_DISCARDER_LOG_ROTATOR_NUM_TO_KEEP_STR))
	}
//	environment {
		// We use this dummy environment variable to load all the properties from the designated file into environment variable (per branch)
//		X_EFRAT_ECHO_DUMMY_ENV_VAR = pipelineCommon.assimilateEnvironmentVariables()

		// Obtain the access token Jenkins uses to connect to GitHub (using a Jenkins credentials ID)
//		GITHUB_ACCESS_TOKEN = credentials('github-demo4echo-access-token-for-reckon-gradle-plugin-id')
//	}
	stages {
//		stage('\u2776 setup \u2728') {
//			steps {
//				sh 'whoami'

//				sh "cp -ar ./${env.COMMON_SUB_MODULE_FOLDER_NAME_ENV_VAR}/.docker /root/.docker"
//				sh "cp -ar ./${env.COMMON_SUB_MODULE_FOLDER_NAME_ENV_VAR}/.kube /root/.kube"
//				sh "cp -ar ./${env.COMMON_SUB_MODULE_FOLDER_NAME_ENV_VAR}/.gradle/gradle.properties /root/.gradle/gradle.properties"
//				sh "cp -ar ./${env.COMMON_SUB_MODULE_FOLDER_NAME_ENV_VAR}/.gradle/init.gradle /root/.gradle/init.gradle"
//			}
//		}
		stage('\u2777 Mark Service For Release \u2728') {
 			failFast true
			parallel {			
				stage ('Mark echobe For Release') {	
					steps {
						build
							job: "echobe/${env.BRANCH_NAME}",
							parameters: [
								string (name: 'TARGET_JENKINSFILE_FILE_NAME', value: 'Jenkinsfile4Release'),
								string (name: 'TARGET_RECKON_SCOPE', value: 'minor'),
								string (name: 'TARGET_RECKON_STAGE', value: 'final')
                		],
							wait: true
						}
					}
				}
				stage ('Mark echofe For Release') {	
					steps {
						build
							job: "echofe/${env.BRANCH_NAME}",
							parameters: [
								string (name: 'TARGET_JENKINSFILE_FILE_NAME', value: 'Jenkinsfile4Release'),
								string (name: 'TARGET_RECKON_SCOPE', value: 'minor'),
								string (name: 'TARGET_RECKON_STAGE', value: 'final')
                		],
							wait: true
						}
					}
				}
			}
		}
	}
	post {
		always {
			echo 'One way or another, I have finished'

			// Do some cleanup
//			sh "rm /root/.gradle/gradle.properties"
//			sh "rm /root/.gradle/init.gradle"
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

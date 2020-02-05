pipeline {
	agent {
			label 'master'
			}

	options { 
		//disableConcurrentBuilds() 
		buildDiscarder(logRotator(numToKeepStr: '10'))
		timestamps()	
		}
	
	 parameters {
		gitParameter(branch: '',
                     branchFilter: 'origin/(.*)',
                     defaultValue: 'master',
                     description: '',
                     name: 'BRANCH',
                     quickFilterEnabled: false,
                     selectedValue: 'NONE',
                     sortMode: 'NONE',
                     tagFilter: '*',
                     type: 'PT_BRANCH')
	  }
	
	stages {

		stage('Prepare') {
				steps {
					script{
						cleanWs()
						def d = new Date().toString().split()
						echo "${d[1..3]}"
						currentBuild.description = "${d[1..3]}-${BRANCH}"
						env.BUILD_DISPLAY_NAME = "${d[1..3]}-${BRANCH}"
						
					}
				}
			}

		stage('Example') {
				steps {
					script{
						echo 'Hello World'
						}
				}
			}
			
		stage('Check env value') {
			steps {
				script{
					echo "branch_name is "+env.BRANCH_NAME
					echo "workspace is "+env.WORKSPACE
					echo "JOB_BASE_NAME is "+env.JOB_BASE_NAME
					echo "JOB name is "+env.JOB_NAME
					echo "build id is "+env.BUILD_ID
					}
			}
		}
		
		stage('Checkout') {
			steps {
				script{
					checkout([$class: 'GitSCM', branches: [[name: "${params.BRANCH}"]], 
						doGenerateSubmoduleConfigurations: false, 
						extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'test']], 
						submoduleCfg: [], 
						userRemoteConfigs: [[credentialsId: 'github_test', 
						url: 'git@github.com:xiangliangliang/test.git']]])
					}
				
					bat label: '', script: '''cd test
									dir'''
			}
		}		
		
	}
	
	post { 
			always { 
				echo 'I will always say Hello again!'
				archiveArtifacts 'test/2020*'
				emailext body: """<p>Jenkins: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                                <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
				subject: "[Jenkins]: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]",
				to: '284604666@qq.com'
			}
		}
}


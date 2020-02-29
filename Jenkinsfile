Skip to content
 
Search or jump to…

Pull requests
Issues
Marketplace
Explore
 
@xiangliangliang 
Learn Git and GitHub without any code!
Using the Hello World guide, you’ll start a branch, write comments, and open a pull request.

Read the guide

xiangliangliang
/
test
 Watch 0
 Star 0  Fork 0
 Code  Issues 0  Pull requests 2  Actions  Projects 0  Wiki  Security  Insights  Settings
Branch: master 
test/Jenkinsfile
Find file Copy path
@xiangliangliang xiangliangliang modify structure
066caa8 now
1 contributor
104 lines (90 sloc)  2.62 KB
RawBlameHistory
    
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
						script{
							//def d = new Date().toString().split()
							println " here is the datetime"
							def d = new Date().format('yyyy-MM-dd HH:mm:ss')
							println d
							BUILD_DISPLAY_NAME = ${d}
						}
						 
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
					/*
					println " ===== > i am in the unused block <=========== "
					checkout([$class: 'GitSCM', branches: [[name: "${params.BRANCH}"]], 
						doGenerateSubmoduleConfigurations: false, 
						extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'test']], 
						submoduleCfg: [], 
						userRemoteConfigs: [[credentialsId: 'github_test', 
						url: 'git@github.com:xiangliangliang/test.git']]])
					*/
					
					
					checkout([$class: 'GitSCM', branches: [[name: "${params.BRANCH}"]], 
						  doGenerateSubmoduleConfigurations: false, 
						  extensions: [[$class: 'CloneOption', honorRefspec: true, noTags: false, reference: '', shallow: true], [$class: 'CheckoutOption']], 
						  submoduleCfg: [], 
						  userRemoteConfigs: [[credentialsId: 'github_test', url: 'git@github.com:xiangliangliang/test.git']]])
				
					bat label: '', script: '''cd test
									dir'''
				}
			}
		}		
		
	
	post { 
			always { 
				echo 'I will always say Hello again!'
				/*
				archiveArtifacts 'test/2020*'
				emailext body: """<p>Jenkins: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                                <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
				subject: "[Jenkins]: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]",
				to: '284604666@qq.com'
				*/
			}
		}
	}
}



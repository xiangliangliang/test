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
	  
	triggers {
	  //pollSCM '''TZ=Asia/Shanghai	H/2 * * * *'''
	  upstream(upstreamProjects: '01_pipeline', threshold: hudson.model.Result.FAILURE)
	}	
	
	stages {

		stage('Prepare') {
				steps {
					script{
						cleanWs()
						def d = new Date().toString().split()
						echo "${d[1..3]}"
						currentBuild.description = "${d[1..3]}-${BRANCH}"
						echo env.BUILD_DISPLAY_NAME
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
				echo "current build status: ${currentBuild.result}"
				archiveArtifacts 'test/2020*'
				emailext body: '''<body leftmargin="8" marginwidth="0" topmargin="8" marginheight="4" offset="0">
    					<table width="95%" cellpadding="0" cellspacing="0"  style="font-size: 11pt; font-family: Tahoma, Arial, Helvetica, sans-serif">
        					<tr>
            					<td>
            						<b><font color="#0B610B">Build Summary</font></b>
            						<hr size="2" width="100%" align="center" />
            					</td>
        					</tr>
        					<tr>
            					<td>
                					<ul>
                    					<li>Project Name: ${PROJECT_NAME}</li>
                    					<li>Build Number: # ${BUILD_NUMBER}</li>
                    					<li>Build Cause: ${CAUSE}</li>
                    					<li>Build Status: ${BUILD_STATUS}</li>
                    					<li>Build Log: <a href="${BUILD_URL}console">${BUILD_URL}console</a></li>
                    					<li>Build Url: <a href="${BUILD_URL}">${BUILD_URL}</a></li>
                    					<li>Project Url: <a href="${PROJECT_URL}">${PROJECT_URL}</a></li>
                					</ul>
            					</td>
        					</tr>
    					</table>
				   </body>''',
				subject: "[Jenkins- ${currentBuild.result}]: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]",
				to: '284604666@qq.com'
			}
		}
}


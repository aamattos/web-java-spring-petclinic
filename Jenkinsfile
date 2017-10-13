
mavenTemplate {
	
		//hipchatSend color: 'YELLOW', credentialId: 'hipchat', failOnError: true, message: "Build Started - ${env.JOB_NAME} ${env.BUILD_NUMBER} (Open)", notify: true, room: '321', sendAs: 'Jenkins', textFormat: true
	
		//COMPILE PHASE
		node('maven') {
			
			stage ('Checkout'){
				checkout scm
			}
					
			stage ('Compile'){
				mavenTemplate.compile()
				// Stash saves the workspace so it can be used in subsequent nodes without duplicating the checkout
				stash includes: '**', name: 'compiled'
			}
		}
			

		//TEST PHASE
		stage ('QA'){			
			parallel(
				UnitTest: {				
					node('maven') {
						unstash 'compiled'
						mavenTemplate.test()
					}				
				}, 

				SonarQube: {				
					node('maven') {
						unstash 'compiled'
						mavenTemplate.dependencyCheck()
						def workspace = manager.build.getEnvVars()["WORKSPACE"]
						echo "WORKSPACEVARIABLE ${workspace}"
						mavenTemplate.sonarqube()
						mavenTemplate.dependencyCheckPublishToJenkins()
					}				
				}
			)		
		}

		//DEPLOY PHASE
		node('maven') {
			stage ('Publish'){
				unstash 'compiled'
				mavenTemplate.publish()
			}
			  
			stage ('Deploy'){
				mavenTemplate.deployTomcat()
			}
			
		}		
		
}
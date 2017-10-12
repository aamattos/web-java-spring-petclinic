
mavenTemplate {
	
		//hipchatSend color: 'YELLOW', credentialId: 'hipchat', failOnError: true, message: "Build Started - ${env.JOB_NAME} ${env.BUILD_NUMBER} (Open)", notify: true, room: '321', sendAs: 'Jenkins', textFormat: true
	
		//COMPILE PHASE
		node('maven') {
			
			stage ('Checkout'){
				checkout scm
			}
					
			stage ('Compile'){
				mavenTemplate.compile()
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
						mavenTemplate.sonarqube()
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
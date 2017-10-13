
mavenTemplate {
	
		//hipchatSend color: 'YELLOW', credentialId: 'hipchat', failOnError: true, message: "Build Started - ${env.JOB_NAME} ${env.BUILD_NUMBER} (Open)", notify: true, room: '321', sendAs: 'Jenkins', textFormat: true
	
		//COMPILE PHASE
		node('maven') {
			
			stage ('Checkout'){
				checkout scm
			}
					
			stage ('Compile'){
				mavenTemplate.compile()
			}
			
		}

		//TEST PHASE
		stage ('QA'){			
			parallel(
				UnitTest: {				
					node('maven') {
						mavenTemplate.test()
					}				
				}, 

				SonarQube: {				
					node('maven') {
						mavenTemplate.sonarqube()
					}				
				}, 

				DependencyCheck: {
					node('maven') {
						mavenTemplate.dependencyCheck()
					}				
				}
			)		
		}

		//DEPLOY PHASE
		node('maven') {
			
			stage ('Publish'){
				mavenTemplate.publish()
			}
			  
			stage ('Deploy'){
				mavenTemplate.deployTomcat()
			}
			
		}		
		
}
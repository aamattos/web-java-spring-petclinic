
mavenTemplate {
	
	try{
		
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
			
			parallel(UnitTest: {
				
				node('maven') {
					mavenTemplate.test()
				}
				
			}, SonarQube: {
				node('maven') {
					mavenTemplate.sonarqube()
				}
				
			})
		
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
		
	
	}finally{
		//archiveArtifacts artifacts: 'target/libs/**/*.jar', fingerprint: true
	}
		
}



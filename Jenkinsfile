
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
					checkout scm
					mavenTemplate.test()
				}
				
			}, SonarQube: {
				node('maven') {
					checkout scm
					mavenTemplate.sonarqube()
				}
				
			})
		
		}
		
		//DEPLOY PHASE
		node('maven') {
			
			stage ('Publish'){
				checkout scm
				mavenTemplate.publish()
			}
			  
			stage ('Deploy'){
				sh "mvn tomcat7:redeploy-only -DskipTests"
			}
			
		}
		
	
	}finally{
		//archiveArtifacts artifacts: 'target/libs/**/*.jar', fingerprint: true
	}
		
}




mavenTemplate {
	
		
		//COMPILE PHASE
		node('maven') {
			
			stage ('Checkout'){
				checkout scm
			}
					
			stage ('Compile'){
				mavenTemplate.compile()
			}
			
			stage('Unit Test'){
				mavenTemplate.test()
			}
			
			stage('SonarQube'){
				withSonarQubeEnv('SonarQube Totta') {
					sh "mvn sonar:sonar"
				}
			}
			
			stage ('Publish'){
				mavenTemplate.publish()
			}
			  
			stage ('Deploy'){
				mavenTemplate.deployTomcat()
			}
			
		}
		
		
}
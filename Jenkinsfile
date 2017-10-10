@Library(['alm-totta-maven-library@develop', 'alm-totta-commons-library@develop']) _

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
				sh "mvn test"
			}
			
			stage('SonarQube'){
				withSonarQubeEnv('SonarQube Totta') {
					sh "mvn -e -X sonar:sonar"
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
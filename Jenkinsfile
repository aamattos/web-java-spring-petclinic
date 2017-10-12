
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

mavenTemplate {
	
		
		//COMPILE PHASE
		node('maven') {
			
			hipchatSend color: 'YELLOW', credentialId: 'hipchat', failOnError: true, message: "Build Started - ${env.JOB_NAME} ${env.BUILD_NUMBER} (Open)", notify: true, room: 'Jenkins', sendAs: 'Jenkins', server: 'hipchat.ci.gsnet.corp', textFormat: true, v2enabled: true
			
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
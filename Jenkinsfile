
mavenTemplate {
	
	try{
		
		//COMPILE PHASE
		node('maven') {
			
			stage ('Checkout'){
				checkout scm
			}
					
			stage ('Compile'){
				sh "mvn compile"
			}
			
		}
		
		//TEST PHASE
		stage ('QA'){
			
			parallel(UnitTest: {
				
				node('maven') {
					sh "mvn test"
				}
				
			}, SonarQube: {
				node('maven') {
					checkout scm
					withSonarQubeEnv('SonarQube Totta') {
						sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
					}
				}
				
			})
		
		}
		
		//DEPLOY PHASE
		node('maven') {
			
			stage ('Publish'){
				sh "mvn clean deploy -DskipTests"
			}
			  
			stage ('Deploy'){
				sh "mvn tomcat7:redeploy-only -DskipTests"
			}
			
		}
		
	
	}finally{
		//archiveArtifacts artifacts: 'target/libs/**/*.jar', fingerprint: true
	}
		
}



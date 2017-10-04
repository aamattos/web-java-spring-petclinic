
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
			
		parallel(UnitTest: {
			
			node('maven') {
				
				stage ('Unit Test'){
					checkout scm
					sh "mvn test"
				}
				
			}
			
		}, SonarQube: {
			node('maven') {

				stage ('SonarQube'){
					checkout scm
					withSonarQubeEnv('SonarQube Totta') {
						sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
					}
				}

			}
			
		})
	
		
		
		//DEPLOY PHASE
		node('maven') {
			
			stage ('Publish'){
				checkout scm
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



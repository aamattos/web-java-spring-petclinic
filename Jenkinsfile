import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException

mavenTemplate{
	
	try{
		
			//DEVELOP PIPELINE
			node('maven') {
				
				stage("Checkout") {
					checkout scm
				}
				
				stage("Compile") {
					mavenTemplate.compile()
				}
				
			}
				
			stage ('QA'){
				parallel(
					UnitTest: {
						node('maven') {

							checkout scm
							mavenTemplate.test()
							
						}
					},
		
					SonarQube: {
						node('maven') {
							
							checkout scm
							mavenTemplate.dependencyCheck()
							
							withSonarQubeEnv('SonarQube Totta') {
								sh(returnStdout: true, script: "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar")
							}
							
						}
					}
				)
			}
		
			//DEPLOY PHASE
			node('maven') {
				
				stage ('Publish'){
					checkout scm
					mavenTemplate.publish()
				}
				  
				stage ('Deploy'){
					mavenTemplate.deployTomcat()
				}
				
			}
			
		}catch(FlowInterruptedException exc){
			
			logger.info(this, "Flow Canceled")
			
		}finally{
			
		}
	
}


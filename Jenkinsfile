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

							stage("Checkout") {
								checkout scm
							}
							
							stage("Unit Test") {
								mavenTemplate.test()
							}
							
						}
					},
		
					SonarQube: {
						node('maven') {
							
							stage("Checkout") {
								checkout scm
							}
							
							stage('Dep. Check') {
								mavenTemplate.dependencyCheck()
							}
							
							stage('Sonarqube') {
								mavenTemplate.sonarqube()
							}
							
							
						}
					}
				)
			}
		
			//DEPLOY PHASE
			node('maven') {
				
				stage("Checkout") {
					checkout scm
				}
				
				stage ('Publish'){
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


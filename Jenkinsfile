@Library(['alm-totta-platform-library@develop', 'alm-totta-commons-library@develop', 'alm-totta-maven-library@develop']) _

import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException
import pt.alm.util.pipeline.DistributionProfile

	try{
		
			//DEVELOP PIPELINE
			node('maven') {

				stage("Checkout") {

					// Clone the project from Git, checkout the branch configured in Jenkins
					mavenPipeline.checkout()

				}

				stage("Compile") {

					// Maven compile
					mavenPipeline.compile()

					// Save workspace
					stash 'compiled'

				}

				stage('Dep. Check') {

					// OWASP dependency check (checks dependencies for known vulnerabilities)
					mavenPipeline.dependencyCheck()
				}
				
				stage ('Unit Tests'){
					mavenPipeline.test()
				}
				
				stage ('SonarQube'){
					mavenPipeline.sonarqube()
				}
				
			}
			
			mavenPipeline.waitForSonar()
			
			//DEVELOP PIPELINE
			node('maven') {

				stage ('Publish'){
					
					// Restore workspace
					unstash 'compiled'

					mavenPipeline.publish(DistributionProfile.LOCAL)
				}

				stage ('Deploy'){

					// Deploy app to Tomcat
					mavenPipeline.deployTomcat()

				}

			}


		}catch(FlowInterruptedException exc){

			print "Flow Canceled"

		}finally{

		}


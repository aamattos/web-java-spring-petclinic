//@Library(['alm-totta-platform-library@develop', 'alm-totta-commons-library@develop', 'alm-totta-maven-library@develop']) _

import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException
import pt.alm.util.pipeline.DistributionProfile

	try{
		
			//DEVELOP PIPELINE
			node('maven') {
				
				stage("Test") {
					
					// Clone the project from Git, checkout the branch configured in Jenkins
					developerHub.publishDeployData()

				}

				stage("Checkout") {

					// Clone the project from Git, checkout the branch configured in Jenkins
					mavenPipeline.checkout()

				}

				//Compile, Dep Check, Tests & Sonarqube
				stage ('QA'){
					mavenPipeline.sonarqube("UNSTABLE")
				}
				
				stage ('Publish'){
					
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


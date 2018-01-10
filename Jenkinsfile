@Library(['alm-totta-platform-library@develop', 'alm-totta-commons-library@develop', 'alm-totta-maven-library@develop']) _

import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException
import pt.alm.util.pipeline.DistributionProfile
import pt.alm.dashboard.model.*

try{
	
		//DEVELOP PIPELINE
		node('maven') {
			
			stage("Checkout") {

				// Clone the project from Git, checkout the branch configured in Jenkins
				mavenPipeline.checkout()
				stash name: 'develop', useDefaultExcludes: false

			}

			//Compile, Dep Check, Tests & Sonarqube
			stage ('QA'){
				mavenPipeline.sonarqube("UNSTABLE")
			}
			
			stage ('Publish'){
				
				mavenPipeline.publish(DistributionProfile.LOCAL)
			}
			

		}
		


}catch(FlowInterruptedException exc){

	print "Flow Canceled"

}finally{

}



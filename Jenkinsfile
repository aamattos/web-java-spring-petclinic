//@Library(['alm-totta-platform-library@develop', 'alm-totta-commons-library@develop', 'alm-totta-maven-library@develop']) _

import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException
import pt.alm.dashboard.model.*

try{
	
		//DEVELOP PIPELINE
		node('maven') {
			
			stage("Checkout") {
				
				sh "git config --global http.sslVerify false"

				// Clone the project from Git, checkout the branch configured in Jenkins
				mavenPipeline.checkout()
				stash name: 'develop', useDefaultExcludes: false

			}

			//Compile, Dep Check, Tests & Sonarqube
			stage ('QA'){
				mavenPipeline.sonarqube("UNSTABLE")
			}
			
			stage ('Publish'){
				
				mavenPipeline.publish()
			}
			
			stage ('Deploy'){
				
				build "deploy"
			}
			

		}
		


}catch(FlowInterruptedException exc){

	print "Flow Canceled"

}finally{

}



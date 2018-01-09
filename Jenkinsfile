@Library(['alm-totta-platform-library@develop', 'alm-totta-commons-library@develop', 'alm-totta-maven-library@develop']) _

import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException
import pt.alm.util.pipeline.DistributionProfile
import pt.alm.dashboard.model.*

try{
		def authToken = "petclinic-openshift-token"
		def templatePath = "openshift/petclinic.yml"
		def petclinic_war_url
	
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
				
				petclinic_war_url = mavenPipeline.publish(DistributionProfile.LOCAL)
				
			}
			
			stage ('Deploy in Dev'){
				
				def params =  readYaml file: "openshift/params-dev.yml"
			    params.petclinic_war_url =  "${petclinic_war_url}"
				
				openshiftPipeline.applyTemplate(authToken, "alm-petclinic-dev", templatePath, params)
				
			}
			
			stage ('Deploy in Pro'){
				
				def params =  readYaml file: "openshift/params.yml"
				
				//TODO Gerar Release
				params.petclinic_war_url =  "${petclinic_war_url}"
	
				openshiftPipeline.applyTemplate(authToken, "alm-petclinic", templatePath, params)
				
			}


		}
		


}catch(FlowInterruptedException exc){

	print "Flow Canceled"

}finally{

}



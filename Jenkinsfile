@Library(['alm-totta-platform-library@develop', 'alm-totta-commons-library@develop', 'alm-totta-maven-library@develop']) _

import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException
import pt.alm.util.pipeline.DistributionProfile
import pt.alm.dashboard.model.*

try{
		def authToken = "petclinic-openshift-token"
		def templateName = "petclinic"
		def templatePath = "openshift/petclinic.yml"
		def devTarget = "alm-petclinic-dev"
		def proTarget = "alm-petclinic"
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
				
				mavenPipeline.publish(DistributionProfile.LOCAL)
				
				//TODO OBTER DO PUBLISH
				petclinic_war_url =  "https://nexus-alm-dev.paas.totta.dev.corp/repository/maven-snapshots/org/springframework/samples/petclinic/1.0.0-SNAPSHOT/petclinic-1.0.0-20180108.160134-40.war"
			}

		}
		
		//Deploy in Dev
		node('openshift') {
		
			stage("checkout") {
				unstash "develop"
			}
			
			def params =  readYaml file: "openshift/params-dev.yml"
		    params.petclinic_war_url =  "${petclinic_war_url}"

			openshiftPipeline.applyTemplate(authToken, devTarget, templateName, templatePath, params)
			
		}


}catch(FlowInterruptedException exc){

	print "Flow Canceled"

}finally{

}



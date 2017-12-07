import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException


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

				}

				stage('Dep. Check') {

					// OWASP dependency check (checks dependencies for known vulnerabilities)
					mavenPipeline.dependencyCheck()

					// Save workspace
					stash 'compiled-depcheck'

				}
				
				stage ('Unit Tests'){
					mavenPipeline.test()
				}
				
				stage ('SonarQube'){
					sh "mvn sonar:sonar"
				}
				
				stage ('Publish'){
					
					// Restore workspace
					unstash 'compiled-depcheck'

					// Publish to local environment Nexus repository
					mavenPipeline.publish('local')
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


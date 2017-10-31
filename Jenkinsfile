import org.jenkinsci.plugins.workflow.steps.FlowInterruptedException

mavenTemplate{

	try{

			//DEVELOP PIPELINE
			node('maven') {

				stage("Checkout") {

					// Checkout the project from Git
					mavenTemplate.checkout()

				}

				stage("Compile") {

					// Maven compile
					mavenTemplate.compile()

				}

				stage('Dep. Check') {

					// OWASP dependncy check (checks dependencies for known vulnerabilities)
					mavenTemplate.dependencyCheck()

					// Save workspace
					stash 'compiled-depcheck'

				}

			}

			// QA - unit testing and SonarQube code analysis executed in parallel
			stage ('QA'){
				parallel(

					UnitTest: {
						node('maven') {

							// Restore workspace
							unstash 'compiled-depcheck'

							// Maven unit testing
							mavenTemplate.test()

						}
					},

					SonarQube: {
						node('maven') {

							// Restore workspace
							unstash 'compiled-depcheck'

							// SonarQube code analysis
							mavenTemplate.sonarqube()

						}
					}
				)
			}

			//DEPLOY PHASE
			node('maven') {

				stage ('Publish'){

					// Restore workspace
					unstash 'compiled-depcheck'

					// Publish to local environment Nexus repository
					mavenTemplate.publish('local')
				}

				stage ('Deploy'){

					// Deploy app to Tomcat
					mavenTemplate.deployTomcat()

				}

			}

		}catch(FlowInterruptedException exc){

			print "Flow Canceled"

		}finally{

		}

}

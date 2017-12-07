@Library('alm-totta-platform-library') _

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

			}

			// QA - unit testing and SonarQube code analysis executed in parallel
			stage ('QA'){
				parallel(

					UnitTest: {
						node('maven') {

							// Restore workspace
							unstash 'compiled-depcheck'

							// Maven unit testing
							mavenPipeline.test()

						}
					},

					SonarQube: {
						node('maven') {

							// Restore workspace
							unstash 'compiled-depcheck'

							// SonarQube code analysis
							mavenPipeline.sonarqube()

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


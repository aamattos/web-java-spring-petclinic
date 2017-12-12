@Library('alm-totta-maven-library') _

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

					// Save workspace
					stash 'compiled'

				}

//				stage('Dep. Check') {
//
//					// OWASP dependency check (checks dependencies for known vulnerabilities)
//					mavenPipeline.dependencyCheck()
//				}
				
				stage ('Unit Tests'){
					sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=false"
					sh "mvn test"
				}
				
				stage ('SonarQube'){
					withSonarQubeEnv('SonarQube Totta') {
						sh "mvn sonar:sonar"
					}
				}
				
			}
			
			// No need to occupy a node
		  timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
			echo "Waiting QualityGate Response"
			def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
			echo "QualityGate Response Received. Status: ${qg.status}"
			if (qg.status != 'OK') {
			  currentBuild.result = 'UNSTABLE'
			}
		  }
			
			//DEVELOP PIPELINE
			node('maven') {

				stage ('Publish'){
					
					// Restore workspace
					unstash 'compiled'

					// Publish to local environment Nexus repository
					//mavenPipeline.publish('local')
					def distSnapshots = "maven-snapshots:default:http://nexus:8081/repository/maven-snapshots"
					def distReleases = "maven-releases:default:http://nexus:8081/repository/maven-releases/"
					
					sh(returnStdout: true, script: "mvn clean deploy -DskipTests -DaltSnapshotDeploymentRepository=${distSnapshots} -DaltReleaseDeploymentRepository=${distReleases}")
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



mavenTemplate {
	
	node('maven') {
		
		  def jenkins_password = sh(returnStdout: true, script: "cat /root/.m2/jenkins_password")
		  
		  echo "jenkins_password: ${jenkins_password}"
		
		  stage ('Checkout'){
			checkout scm
		  }
				
		  stage ('Compile'){
			mavenPipeline.compile()
		  }
		
		  stage ('QA'){
		
			parallel(UnitTest: {
				mavenPipeline.test()
			}, SonarQube: {
				mavenPipeline.sonarqube()
			})
		
		  }
		
		  stage ('Publish'){
			mavenPipeline.publish()
		  }
		  
		  stage ('Deploy'){
			 sh "mvn tomcat7:redeploy-only -DskipTests"
		  }
		}
}



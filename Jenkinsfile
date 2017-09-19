
mavenTemplate {
	
	node('maven') {
		
		  def settings = sh(returnStdout: true, script: "cat /root/.m2/settings.xml")
		  
		  echo "settings.xml contents: ${settings}"
		
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



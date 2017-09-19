
mavenTemplate {
	
	node('maven') {
		
		  def whoami = sh(returnStdout: true, script: "id")
		  
		  echo "whoami: ${whoami}"
		
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



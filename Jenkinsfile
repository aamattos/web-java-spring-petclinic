
mavenTemplate {
	
	node('maven') {
		
		  def master_pswd = sh(returnStdout: true, script: "echo $MASTER_PASSWORD")
		  
		  echo "master_pswd: ${master_pswd}"
		
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



import pt.alm.util.maven.MavenPipeline

mavenTemplate {
	
	node('maven') {
		
		  def mvnPipeline = new MavenPipeline()
		
		  stage ('Checkout'){
			checkout scm
		  }
				
		  stage ('Compile'){
			mvnPipeline.compile()
		  }
		
		  stage ('QA'){
		
			parallel(UnitTest: {
				mvnPipeline.test()
			}, SonarQube: {
				mvnPipeline.sonarqube()
			})
		
		  }
		
		  stage ('Publish'){
			mvnPipeline.publish()
		  }
		  
		  stage ('Deploy'){
			 sh "mvn tomcat7:redeploy-only -DskipTests"
		  }
		}
}



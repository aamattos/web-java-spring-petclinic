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
			 sh 'mvn test'
			}, SonarQube: {
			withSonarQubeEnv('SonarQube Totta') {
			  sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
			}
			})
		
		  }
		
		  stage ('Publish'){
			sh "whereis mvn"  
			  
			sh "mvn clean deploy -DskipTests"
		  }
		  
		  stage ('Deploy'){
			 sh "mvn tomcat7:redeploy-only -DskipTests"
		  }
		}
}




mavenTemplate {
	
	node('maven') {
		
		  stage ('Checkout'){
			checkout scm
		  }
		
		
		  stage ('Compile'){
			sh "mvn  compile"
		  }
		
		  stage ('QA'){
		
			parallel(UnitTest: {
			 sh 'mvn test'
			}, SonarQube: {
			withSonarQubeEnv('SonarQube Totta') {
			  sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar -Dsonar.host.url=https://sonarqube-alm-dev.tottapaas.totta.dev.corp:9000'
			}
			})
		
		  }
		
		  stage ('Publish'){
			sh "mvn clean deploy -DskipTests"
		  }
		  
		  stage ('Deploy'){
			 sh "mvn tomcat7:redeploy-only -DskipTests"
		  }
		}
}



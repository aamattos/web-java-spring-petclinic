
mavenTemplate {
	
	node('maven') {

			stage ('postgresql test'){				
				testSQL{
					dbUrl = "jdbc:postgresql://PDTOALMD.TOTTA.DEV.CORP:60145/gitlab-dev"
					dbUser = "alm"
					dbPassword = "password"
					dbDriver   = "org.postgresql.Driver"
				}
				
			}
		
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



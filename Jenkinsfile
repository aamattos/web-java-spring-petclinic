
mavenTemplate {
	
	node('maven') {

			stage ('postgresql test'){
//				Class.forName("org.postgresql.Driver")
//				def dbUrl      = "jdbc:postgresql://PDTOALMD.TOTTA.DEV.CORP:60145/gitlab-dev"
//				def dbUser     = "alm"
//				def dbPassword = "password"
//				def dbDriver   = "org.postgresql.Driver"				
//				def sql = Sql.newInstance(dbUrl, dbUser, dbPassword, dbDriver)				
//				def rows = sql.execute "select count(*) from users;"
//				echo rows.dump()
				
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
			sh "mvn clean deploy -DskipTests"
		  }
		  
		  stage ('Deploy'){
			 sh "mvn tomcat7:redeploy-only -DskipTests"
		  }
		}
}



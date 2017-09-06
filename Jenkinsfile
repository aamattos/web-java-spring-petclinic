import groovy.sql.Sql

mavenTemplate {
	
	node('maven') {

			stage ('postgresql test'){
				Class.forName("org.postgresql.Driver")
				def sql = Sql.newInstance("jdbc:postgresql://PDTOALMD.TOTTA.DEV.CORP:60145/gitlab-dev", "alm","password", "org.postgresql.Driver")
				def rows = sql.execute "select count(*) from users;"
				echo rows.dump()
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
			  sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar -Dsonar.host.url=http://sonarqube.alm-dev.svc:9000'
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



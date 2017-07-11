node('jenkins-slave-maven-totta') {

  stage ('Checkout'){
	checkout scm
  }

  stage ('Compile'){
    sh "mvn  compile"
  }

  stage ('QA'){

    parallel(UnitTest: {
    //sh "mvn test"
    }, SonarQube: {
    withSonarQubeEnv('SonarQube Totta') {
      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
    }
    })

  }

  stage ('Release'){
    sh "mvn clean deploy -DskipTests"
  }  
  
  stage ('Deploy'){
     sh "mvn tomcat7:redeploy-only -DskipTests"
  }  
}

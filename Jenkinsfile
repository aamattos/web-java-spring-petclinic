node {
  def mvnHome = tool 'maven-3.3.9'  
  
  stage ('Checkout'){
  git url: 'https://github.com/aamattos/web-java-spring-petclinic'
  sh "git clean -f && git reset --hard origin/mysql"
  }

//  stage ('Compile'){
//  sh "${mvnHome}/bin/mvn compile"
//  } 

//  stage ('QA'){
//    sh "${mvnHome}/bin/mvn sonar:sonar"
//  }

//  stage ('Build'){
//    sh "${mvnHome}/bin/mvn clean install -DskipTests"
//  }

  stage ('Package'){
    sh "${mvnHome}/bin/mvn deploy -DskipTests"
  }  
}

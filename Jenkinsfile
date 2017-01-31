node {
  def mvnHome = tool 'maven-3.3.9'  
  
  stage ('Checkout'){
  git url: 'https://github.com/aamattos/web-java-spring-petclinic'
  sh "git clean -f && git reset --hard origin/mysql"
  }

  stage ('Compile'){
 //   sh "${mvnHome}/bin/mvn compile"
  } 

  stage ('QA'){
 //   sh "${mvnHome}/bin/mvn sonar:sonar"
  }

  stage ('Build'){
    sh "${mvnHome}/bin/mvn clean install -DskipTests"
  }

  stage ('Release'){
  //  sh "${mvnHome}/bin/mvn deploy -DskipTests"
  }  
  
  stage ('Deploy'){
     //sh "${mvnHome}/bin/mvn tomcat7:deploy-only -DskipTests -e -X"
     sh "curl --upload-file target/petclinic-1.0.0-SNAPSHOT.war http://jenkins:jenkins@10.180.254.242:8888/manager/text/deploy?path=/petclinic&update=true"
  }  
}

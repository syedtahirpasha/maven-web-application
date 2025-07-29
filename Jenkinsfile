pipeline{
agent{
node{
label 'node1'
}
	
}

tools{
maven 'maven3.9.10'

}

triggers{
githubPush()
}

options{
timestamps()

}

stages{

  stage('CheckOutCode'){
    steps{
    git branch: 'development', credentialsId: 'eb19accf-4694-4e18-8c63-c23da4d67eab', url: 'https://github.com/syedtahirpasha/maven-web-application.git'
	
	}
  }
  
  stage('Build'){
  steps{
  sh  "mvn clean package"
  }
  }

 stage('ExecuteSonarQubeReport'){
  steps{
  sh  "mvn clean sonar:sonar"
  }
  }
  
  stage('UploadArtifactsIntoNexus'){
  steps{
  sh  "mvn clean deploy"
  }
  }
  
  stage('DeployAppIntoTomcat'){
  steps{
  sshagent(['8f1cd029-06b5-4456-97b2-30fc981f3872']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.0.71.194:/opt/apache-tomcat-9.0.52/webapps/"    
  }
  }
  }
  
}

post{

 success{
 emailext to: 'nayeemstpk1@gmail.com',
          subject: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'devopstrainingblr@gmail.com'
 }
 
 failure{
 emailext to: 'nayeemstpk1@gmail.com',
          subject: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          body: "Pipeline Build is over .. Build # is ..${env.BUILD_NUMBER} and Build status is.. ${currentBuild.result}.",
          replyTo: 'devopstrainingblr@gmail.com'
 }
 
}//stages closing


}//Pipeline closing

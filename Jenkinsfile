node {
   
   def maveenHome = tool name: "Maveen3.8.5"
   
   properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])]) 
   // this prioeprtey is to discard past buillds and poll scm.
   
   stage ('checkoutCode') {
      git branch: 'Development', url: 'https://github.com/VenkateshTondapu/maven-web-application.git'
   } 
   
   stage ('Build') {
       sh "${maveenHome}/bin/mvn clean package"
   }
   
    stage ('SonarCheck') { // this stage is for sonar scan.
		 sh "${maveenHome}/bin/mvn sonar:sonar"
   }
   
   stage ('Publish Artifactory') { // this stage is to deploy the artifactory 
		 sh "${maveenHome}/bin/mvn deploy"
	}
	
	stage ('tomcat Deployment') { // This stage is to deploy war file into tomcat
	    sshagent(['3460080c-202d-4214-8088-4700b9d55273']) {
			sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.34.225:/opt/apache-tomcat-9.0.76/webapps"}
	}
	
    
}

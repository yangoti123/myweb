node {
   
   stage('SCM Checkout'){
    // Clone repo
	git branch: 'master', 
	credentialsId: 'github', 
	url: 'https://github.com/yangoti123/myweb'
   
   }
	
   stage('Mvn Package'){
	   // Build using maven
	   def mvn = tool (name: 'maven3', type: 'maven') + '/bin/mvn'
	   sh "${mvn} clean package"
   }
   
   stage('deploy-dev'){
       def tomcatDevIp = '172.31.23.143'
	   def tomcatHome = '/opt/tomcat/'
	   def webApps = tomcatHome+'webapps/'
	   def tomcatStart = "${tomcatHome}bin/startup.sh"
	   def tomcatStop = "${tomcatHome}bin/shutdown.sh"
	   
	   sshagent (credentials: ['tomcat-baskar']) {
	      sh "scp -o StrictHostKeyChecking=no target/myweb*.war ec2-user@${tomcatDevIp}:${webApps}myweb.war"
          sh "ssh ec2-user@${tomcatDevIp} ${tomcatStop}"
		  sh "ssh ec2-user@${tomcatDevIp} ${tomcatStart}"
       }
   }
   stage('Email Notification'){
		mail bcc: '', body: '''Hi Team, You build successfully deployed

Thanks,
DevOps Team''', cc: '', from: '', replyTo: '', subject: 'Build Success', to: 'ybr4b9@gmail.com'
   
   }
}

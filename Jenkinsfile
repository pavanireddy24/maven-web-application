node{
    
    def mavenHome = tool name:"maven3.8.1"
  	      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
	      echo "Jenkins Node Name ${env.NODE_NAME}"
	  
	      echo "Jenkins Home ${env.JENKINS_HOME}"
	      echo "Jenkins URL ${env.JENKINS_URL}"
	      echo "JOB Name ${env.JOB_NAME}"

   stage('CheckOutCode')
    {
        git branch: 'development', credentialsId: '0e8e40b5-7cb6-4503-aee6-952d87dc80bb', url: 'https://github.com/pavanireddy24/maven-web-application.git'
    }
    
    
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('SonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
        
    }
    stage('UploadArtifactIntoNexus')
    {
        sh "${mavenHome}/bin/mvn clean deploy"
    }
    
    stage('DeployAppIntoTomcatServer')
    {
        sshagent(['464904d7-6efc-48a7-b1a1-36019b9eca1b']) {
    sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.169.164:/opt/apache-tomcat-9.0.52/webapps/"
        }
}
stage('SendEmailNotification'){
    emailext body: '''Build is over 

Regards,
Pavani Reddy''', subject: 'Build Over...', to: 'pavani.v024@gmail.com'
}
    }

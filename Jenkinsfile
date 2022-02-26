node
{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: "maven3.8.4"   
    
    stage('CheckOutCode'){
        git branch: 'development', 
        credentialsId: '8dd6c89c-f5dd-4563-9342-03c78b0457cb', 
        url: 'https://github.com/rss-apps-projects/maven-web-application.git'
    }
    
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
  
   
    stage('ExecuteSonarQubeReport'){
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }
    
    stage('UploadArtifactIntoNexusRepo'){
        sh "${mavenHome}/bin/mvn clean deploy"
    }
    
    stage('DeployApplicationIntoTomcat'){
        sshagent(['3b4d9247-d2d2-4048-83a6-cc559ec8065f']) {
         sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.3.179:/opt/apache-tomcat-9.0.58/webapps"
        }
    }
    
  
    stage('SendEmailNotification'){
        mail bcc: 'devopstraningblr@gmail.com', body: '''Build Succeeded !!!

        Regards
        Rajas Technologies''', cc: 'devopstraningblr@gmail.com', from: '', 
        replyTo: '', subject: 'Build Status', to: 'devopstraningblr@gmail.com'
    }
}

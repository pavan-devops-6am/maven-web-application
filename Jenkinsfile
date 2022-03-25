node{
   
def mavenHome = tool name: "maven 3.8.5"
buildName 'Dev - ${BUILD_NUMBER}' 
buildDescription 'Pipeline Script - Scriptedway'
 

echo "The Node name is:  ${env.NODE_NAME} "
echo "The Job name is:  ${env.JOB_NAME} "
echo " The Build number is: ${env.BUILD_NUMBER}  "

//properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])
//properties([[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
 
 
//Checkout Stage
stage('CheckoutCode'){
  
   git branch: 'development', credentialsId: 'b72793b7-4209-4da2-b3fb-2b91757fb6a9', url: 'https://github.com/pavan-devops-6am/maven-web-application.git'
}

//Build Stage
stage('Build'){
   
sh "$mavenHome/bin/mvn clean package"
}


//Generate SonarQube Report 
stage('SonarQubeReport'){
sh "$mavenHome/bin/mvn sonar:sonar"
}

//Upload Artifact into Artifcatory repo
stage('UploadArtifcatsIntoNexus'){
sh "$mavenHome/bin/mvn deploy"
}


//Deploy App into Tomcat Server
stage('DeployAppIntoTomcat'){
sshagent(['7f303290-28a0-466e-af6d-198cd5de0182']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.206.37:/opt/apache-tomcat-9.0.59/webapps"
}
}


}//Node Closing 

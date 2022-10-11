node{
 echo "Job name is : ${env.JOB_NAME}"
 echo "Node name is : ${env.NODE_NAME}"
 echo "Build number is : ${env.BUILD_NUMBER}"
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
 def mavenHome = tool name: 'maven3.8.6'
 stage('CheckOutCode'){
  git branch: 'development', credentialsId: 'a9458a4d-2310-4797-80ec-f600ac039905', url: 'https://github.com/saleelmuhammed/maven-web-application.git'
 }

 stage('Build'){
  sh "${mavenHome}/bin/mvn clean package"
  //sh "mvn clean package"
 }
 stage('Execute SonarQubeReports'){
  sh "${mavenHome}/bin/mvn clean sonar:sonar"
  }
  stage('Deploy ArtifactsintoNexus'){
  sh "${mavenHome}/bin/mvn clean deploy"
 }
  stage('Deploy AppintoTomcat'){
  sshagent(['cf7d5ca2-8184-47d8-92b0-a13a4cd1859a']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.14.128:/opt/apache-tomcat-9.0.65/webapps/"
    
}
}
}

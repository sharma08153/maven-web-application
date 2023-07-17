node{
echo "Jobe name is: ${env.JOB_NAME}"
echo "Build number is: ${env.BUILD_NUMBER}"
echo "Node name is: ${env.NODE_NAME}"
echo "Jenkins home dir is: ${env.JENKINS_HOME}"  
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])    
def mavenHome=tool name="maven3.9.3"

stage('CheckoutCode'){
git branch: 'development', credentialsId: '283bb7b7-b30b-4346-8e95-49599fef3892', url: 'https://github.com/sharma08153/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExcuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar package"
}

stage('UploadArtifatIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcatServer'){
sshagent(['09e882e8-40f5-4879-b2fc-a64339ff4939']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.7.253.30:/opt/apache-tomcat-9.0.78/webapps/"
}
}

}

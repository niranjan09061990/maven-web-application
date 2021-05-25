node
{
    def mavenHome = tool name: "maven3.81.1"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
stage ('Checkoutcode')
{
git branch: 'development', url: 'https://github.com/niranjan09061990/maven-web-application.git'
}
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
    /*
stage('Sonarcube report')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}
*/
stage('uploadartifactintonexus')
{
sh "${mavenHome}/bin/mvn deploy"
}
stage('deployintotomcatserver')
{
sshagent(['78838617-3342-431c-bb55-5f0bcef3dcf6']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.0.178.20:/opt/apache-tomcat-9.0.45/webapps/"
}
}
stage('sendemailnotification')
{
emailext body: '''Build over

Regards,
Niranjan''', subject: 'Build Status', to: 'niranjanjkp@gmail.com'
}
}

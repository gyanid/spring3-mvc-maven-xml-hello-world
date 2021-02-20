pipeline{
    agent any
      tools{
         //install the maven version configured as "M3" and add it to the path
         maven "maven3"


      }
      environment {
        EMAIL_TO = 'gyan.csc@gmail.com'
    }
    stages{
      stage('SCM'){
        steps{
            // some code from repository
            git credentialsId: 'GITID', url: 'https://github.com/gyanid/spring3-mvc-maven-xml-hello-world.git' 
        }
      }
      stage('Build'){
       steps{
           //maven package
           sh 'mvn package'
         }
      }
      stage('Artifact'){
       steps{
           //Artifact package
           archiveArtifacts 'target/*.war'
         }
      }
      stage('deploy'){
        steps{
            withCredentials([usernameColonPassword(credentialsId: 'tomcat_credential_ID', variable: 'tomcatcred')]) {
                sh "curl -v -u ${tomcatcred} -T /var/lib/jenkins/workspace/tomcat_deploy_pipeline/target/spring3-mvc-maven-xml-hello-world-3.0-SNAPSHOT.war 'http://ec2-13-232-107-191.ap-south-1.compute.amazonaws.com:8081/manager/text/deploy?path=/pipeline_gyanh&update=true'"
            }
        }
      }
    }
   post {
        

       failure {
            emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        }
        unstable {
            emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Unstable build in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        }
        changed {
            emailext body: 'Check console output at $BUILD_URL to view the results.', 
                    to: "${EMAIL_TO}", 
                    subject: 'Jenkins build is back to normal: $PROJECT_NAME - #$BUILD_NUMBER'
        }
       success {
            emailext body: 'Check console output at $BUILD_URL to view the results.', 
                    to: "${EMAIL_TO}", 
                    subject: 'Jenkins build is back to normal: $PROJECT_NAME - #$BUILD_NUMBER'
        }
    }


 }

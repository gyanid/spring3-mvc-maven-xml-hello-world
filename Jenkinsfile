pipeline{
    agent any
      tools{
         //install the maven version configured as "M3" and add it to the path
         maven "maven3"
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
                sh "curl -v -u ${tomcatcred} -T /var/lib/jenkins/workspace/tomcat_deploy_pipeline/target/spring3-mvc-maven-xml-hello-world-3.0-SNAPSHOT.war 'http://ec2-35-154-133-207.ap-south-1.compute.amazonaws.com:8081/manager/text/deploy?path=/pipeline_gyanh&update=true'"
            }
        }
      }
    }
   post {
        failure {
            script {
                currentBuild.result = 'FAILURE'
            }
        }

        always {
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "gyan.csc@gmail.com,gyan.csc",
                sendToIndividuals: true])
        }
    }


 }

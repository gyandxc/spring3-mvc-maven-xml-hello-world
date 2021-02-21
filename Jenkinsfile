pipeline {
   agent any

   tools {
      // Install the Maven version configured as "M3" and add it to the path.
      maven "maven3"
   }

   stages {
      stage('Build') {
         steps {
            // Get some code from a GitHub repository
            //git branch: 'test', changelog: false, credentialsId: 'git_jms_cre', poll: false, url: 'https://github.com/gyandxc/spring3-mvc-maven-xml-hello-world.git'
              git credentialsId: 'gyanid1IDgitdxc', url: 'https://github.com/gyandxc/spring3-mvc-maven-xml-hello-world.git'
            // Run Maven on a Unix agent.
            sh "mvn clean"
	    sh "mvn package"
            // To run Maven on a Windows agent, use
            // bat "mvn -Dmaven.test.failure.ignore=true clean package"
         }
        post {
            // If Maven was able to run the tests, even if some of the test
            // failed, record the test results and archive the jar file.
            success {
               archiveArtifacts 'target/*.war'
            }
			}

      }
      stage('deploy'){
          steps {
              sh "sudo cp /var/lib/jenkins/workspace/tomcat_deploy_pipeline/target/spring3-mvc-maven-xml-hello-world-1.0-SNAPSHOT.war /var/lib/tomcat/webapps/"
          }
       
    }
    stage ('success'){
            steps {
                script {
                    currentBuild.result = 'SUCCESS'
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

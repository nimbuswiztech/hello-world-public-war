pipeline{
        agent {
                docker {
                image 'maven'
                // args '-v $HOME/.m2:/root/.m2'
                args '-u root'
                }
        }
        stages{
              stage('Quality Gate Statuc Check'){
                  steps{
                      script{
                      withSonarQubeEnv('sonarserver') { 
                      sh "mvn sonar:sonar"
                       }
                      timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
		    sh "mvn clean install"
                  }
                }  
              }
            stage("deploy"){
            steps{
              sshagent(['Tomcat']) {
                 sh "scp -o StrictHostKeyChecking=no webapp/target/webapp.war ubuntu@3.84.22.129:/opt/tomcat/webapps"
                    }
                }
            }
        }
}

// pipeline {
//     agent any
//     environment {
//         PATH = "/opt/apache-maven-3.6.3/bin:$PATH"
//     }
//     stages {
//         stage("clone code"){
//             steps{
//                git credentialsId: 'git_credentials', url: 'https://github.com/ravdy/hello-world.git'
//             }
//         }
//         stage("build code"){
//             steps{
//               sh "mvn clean install"
//             }
//         }
//         stage("deploy"){
//             steps{
//               sshagent(['Tomcat']) {
//                  sh "scp -o StrictHostKeyChecking=no webapp/target/webapp.war ubuntu@3.84.22.129:/opt/tomcat/webapps"
                 
//                 }
//             }
//         }
//     }
// }


pipeline{  
agent any
    environment {
        PATH ="$PATH:/opt/apache-maven-3.9.6/bin"
    }
    stages{
       stage('Checkout'){
            steps{
                git 'git@github.com:githubswap86/Sonar-Project.git'
	    }
	  }      
	    stage('package'){
            steps{
                sh 'mvn clean package'
            }  
          }
	    stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('SonarQube') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn clean verify sonar:sonar -Dsonar.projectKey=sonar-project -Dsonar.host.url=http:3.84.109.147:9000 -Dsonar.login=sqp_52083b0f26ca2f052941158828cf8f3a661c06c0"
    }
        }
        }
       
    stage ('Deploy to Nexus') {
            steps {
      nexusArtifactUploader(
      nexusVersion: 'nexus3',
      protocol: 'http',
      nexusUrl: '3.88.217.139:8081',
      groupId: 'myGroupId',
      version: '1.0-SNAPSHOT',
      repository: 'maven-snapshots',
      credentialsId: 'nexuscredentials',
      artifacts: [
      [artifactId: 'maven-project',
      classifier: '',
      file: 'webapp/target/webapp.war',
      type: 'war']
      ])
      }
        }
        stage ('Deploy to Prod'){
     steps {
        sh 'scp -o StrictHostKeyChecking=no webapp/target/webapp.war root@54.81.135.178:/opt/apache-tomcat-8.0.52/webapps'
          }
      }
    }
post {
        success {
            emailext (
                subject: "Pipeline Success: ${env.JOB_NAME}",
                body: "The pipeline ${env.JOB_NAME} has successfully completed.",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                to: "manchala.ksvc@gmail.com",
                replyTo: "manchala.ksvc@gmail.com",
                from: "manchala.ksvc@gmail.com",
                attachLog: true,
                compressLog: true
            )
        }
        failure {
            emailext (
                subject: "Pipeline Failure: ${env.JOB_NAME}",
                body: "The pipeline ${env.JOB_NAME} has failed. Please check the Jenkins console output for more details.",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                to: "manchala.ksvc@gmail.com",
                replyTo: "manchala.ksvc@gmail.com",
                from: "manchala.ksvc@gmail.com",
                attachLog: true,
                compressLog: true
            )
        }
    }
		
		    }

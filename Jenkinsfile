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
       stage('Package'){
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
        sh "mvn clean verify sonar:sonar -Dsonar.projectKey=sonar-project -Dsonar.host.url=http://35.172.219.11:9000 -Dsonar.login=sqp_52083b0f26ca2f052941158828cf8f3a661c06c0"
    }
        }
        }
       
    stage ('Deploy to Nexus') {
            steps {
      nexusArtifactUploader(
      nexusVersion: 'nexus3',
      protocol: 'http',
      nexusUrl: '54.166.62.126:8081',
      groupId: 'myGroupId',
      version: '1.0-SNAPSHOT',
      repository: 'maven-snapshots',
      credentialsId: 'nexuscredentails',
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
        sh 'scp -o StrictHostKeyChecking=no webapp/target/webapp.war root@18.232.79.243:/opt/apache-tomcat-8.0.52/webapps'
           }
   }
}    
}

pipeline{
    agent any
    environment {
        PATH = "$PATH:/usr/share/maven/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/gundepallieswartej/jenkins-docker-example.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
         }
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('sonarqube-8.9.9') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn sonar:sonar"
    }
        }
        }
        stage('Build image') {
       dockerImage = docker.build("gundepallieswartej/my-app-1.0")
    }
    
        stage('Push image') {
        withDockerRegistry([ credentialsId: "dockerhubcred", url: "https://hub.docker.com/repository/docker/eswargundepalli/mydokerimage" ]) {
        dockerImage.push()
        }
        }
       
    }
}

pipeline {
    agent any
	 def server = Artifactory.server "Artifactory Version 4.15.0"
    // Create an Artifactory Maven instance.
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/shashinandankumar/jenkintest.git"
            }
        }
		stage('SonarQube analysis'){
		
		steps {
               withSonarQubeEnv('SonarQube6.3'){
			   
			   bat 'mvn org.sonarsource.scanner.maven:sonarqube-maven-plugin:3.3.0.603:sonar'
			   }
			   
            }
		
		}
		
		 stage('Artifactory configuration') {
        // Tool name from Jenkins configuration
        //rtMaven.tool = "Maven-3.3.9"
        // Set Artifactory repositories for dependencies resolution and artifacts deployment.
        rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
    }

    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean install'
    }

    stage('Publish build info') {
        server.publishBuildInfo buildInfo
    }
		}
		
		
		}

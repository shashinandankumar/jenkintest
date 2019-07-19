pipeline {
    agent any
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
		
		}
		
		
		}
pipeline{
	agent any
	stages {
		stage('Build Backend'){
			steps{
				bat 'mvn clean package -DskipTest=true'
				}
		}
		stage('Unit Tests'){
			steps{
				bat 'mvn test'
				}
		}
		stage('Sonar Analysis'){
			environment{
				scannerHome = tool 'SONAR_SCANNER'
			}
			steps{
				withSonarQubeEnv('SONAR_DOCKER'){
					echo "${scannerHome}/bin/sonar-scanner.bat"
					bat "\"${scannerHome}\\bin\\sonar-scanner.bat\""
					
				}
			}
		}
	}
}
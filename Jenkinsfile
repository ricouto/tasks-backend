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
					bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.99.100:9000 -Dsonar.login=f38b30956a2d15cb0e794c1dabddfe3f5eefd38d -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=.mvn/wrapper/MavenWrapperDownloader.java,src/main/java/br/ce/wcaquino/taskbackend/TaskBackendApplication.java,src/main/java/br/ce/wcaquino/taskbackend/model/Task.java,src/test/java/br/ce/wcaquino/taskbackend/controller/TaskControllerTest.java,src/test/java/br/ce/wcaquino/taskbackend/utils/DateUtilsTest.java"
				}
			}
		}
		stage('Quality Gate'){
			steps{
				timeout(time: 1, unit: 'MINUTES'){
					waitForQualityGate abortPipeline: true
				}
			}
		}
	}
}
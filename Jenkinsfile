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
				sleep(8)
				timeout(time: 1, unit: 'MINUTES'){
					waitForQualityGate abortPipeline: true
				}
			}
		}
		stage('Deploy Backend'){
			steps{
				deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
			}
		}
		stage('API Test'){
			steps{
				dir('api-test') {
					git credentialsId: 'github_login', url: 'https://github.com/ricouto/tasks-api-test'
					bat 'mvn test'
				}
			}
		}
		stage('Deploy Frontend'){
			steps{
				dir('frontend') {
					git credentialsId: 'github_login', url: 'https://github.com/ricouto/tasks-frontend'
					bat 'mvn clean package'
					deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
				}
			}
		}
		stage('Functional Test'){
			steps{
				dir('functional-test') {
					git credentialsId: 'github_login', url: 'https://github.com/ricouto/tasks-fuctional-test'
					bat 'mvn test'
				}
			}
		}
		stage('Deploy Prod'){
		environment{
				dockerHome = 'C:\\Program Files\\Docker Toolbox'
			}
			steps{
				echo "${dockerHome}\\docker-compose.exe"
				echo "bat \"${dockerHome}\\docker-compose.exe\" build ."
				echo "bat \"${dockerHome}\\docker-compose.exe\" up -d"
				bat "docker-compose build"
				bat "docker-compose up -d"
			}
		}		
	}
}
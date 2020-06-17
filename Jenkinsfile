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
	}
}
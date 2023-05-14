node(){

	def sonarHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
	
	stage('Code Checkout'){
		checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHubCreds', url: 'https://github.com/anujdevopslearn/MavenBuild']])
	}
	stage('Build Automation'){
		withMaven(maven: 'Maven 3.9.2') {
		    sh """
			ls -lart
			mvn clean install
			ls -lart target

			"""
        	}
	}
	
	stage('Code Scan'){
		withSonarQubeEnv(installationName: 'My SonarQube Server') { // You can override the credential to be used
	      		sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
	}
		/*withSonarQubeEnv(credentialsId: 'SonarQubeCreds') {
			sh "${sonarHome}/bin/sonar-scanner"
		}*/
		
	}
	
	stage('Code Deployment'){
		deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://3.17.203.8:8080/')], contextPath: 'counterwebapp', onFailure: false, war: 'target/*.war'
	}
}

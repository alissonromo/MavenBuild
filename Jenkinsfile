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
		/*def scannerHome = tool 'SonarScanner';
		    withSonarQubeEnv('My SonarQube Server') { // If you have configured more than one global server connection, you can specify its name
		      sh "${scannerHome}/bin/sonar-scanner"
		    }
		withSonarQubeEnv(credentialsId: 'SonarQubeCreds') {
			sh "${sonarHome}/bin/sonar-scanner"
		}*/
		
	}
	
	stage('Code Deployment'){
		deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://18.223.160.36:8080/')], contextPath: 'counterwebapp', onFailure: false, war: 'target/*.war'
	}
}

node {

stage('SCM') {
git 'https://github.com/asquarezone/game-of-life.git'
}

stage('Build Step')
	{
		withMaven(globalMavenSettingsConfig: 'MavenSettings', maven: 'Maven3', mavenSettingsConfig: '77f3423a-ba06-4dda-946d-cef13e237611', tempBinDir: '') 
		{
			configFileProvider([configFile(fileId: '77f3423a-ba06-4dda-946d-cef13e237611', variable: 'Maven_settings')]) 
			{
				properties([gitLabConnection(''), parameters([string(defaultValue: '1.0', description: '', name: 'SERVE_VERSION_NUMBER', trim: false)])])
				sh 'echo ${SERVE_VERSION_NUMBER}'
				sh 'mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent versions:set -DnewVersion=${SERVE_VERSION_NUMBER}'
			}
		}
	}
	stage('SonarQube analysis')
	{
		withMaven(globalMavenSettingsConfig: 'MavenSettings', maven: 'Maven3', mavenSettingsConfig: '77f3423a-ba06-4dda-946d-cef13e237611', tempBinDir: '')
		{
		        sh 'mvn -s $Maven_settings clean versions:set -DnewVersion=${SERVE_VERSION_NUMBER}'
				sh 'mvn install sonar:sonar -Dsonar.host.url=http://sonarqube-core:9000/sonarqube -Dsonar.login=a22c81b6684773e441e7892446ec6e5eb82e81b2'
			    sh 'curl -v -u vyenmang:Z#8ib9tr --upload-file target/gameoflife-${SERVE_VERSION_NUMBER}.war http://nexus3-core:8081/nexus3/repository/maven-releases/com/productionline/gameoflife-${SERVE_VERSION_NUMBER}.war'
		}
	}
 /*stage('Deploy to Nexus3'){
 	withMaven(globalMavenSettingsConfig: 'MavenSettings', maven: 'Maven3', mavenSettingsConfig: '77f3423a-ba06-4dda-946d-cef13e237611', tempBinDir: '')
		{
 sh 'curl -v -u vyenmang:Z#8ib9tr --upload-file target/gameoflife-${SERVE_VERSION_NUMBER}.${BUILD_NUMBER}.war http://nexus3-core:8081/nexus3/repository/maven-releases/com/productionline/gameoflife-${SERVE_VERSION_NUMBER}.${BUILD_NUMBER}.war'
 }
 } */
}

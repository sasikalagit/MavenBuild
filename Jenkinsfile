node('') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Sonar Analysis'){
		//sh 'mvn sonar:sonar -Dsonar.host.url=http://35.153.67.119:9000 -Dsonar.login=77467cfd2653653ad3b35463fbfdb09285f08be5'
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage ('Deployment'){
		deploy adapters: [tomcat9(credentialsId: 'deployer', url: 'http://ec2-54-236-49-248.compute-1.amazonaws.com:8080/')], war: 'target/*.war'
	}
	
	stage ('Notification'){
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "build-alerts@example.com"
		    )
	}
}

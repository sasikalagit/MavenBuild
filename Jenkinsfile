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
	
        stage('Publish test results'){
                junit '**/test-results/test/*.xml'
        }
  
	stage ('Sonar Analysis'){
		//sh 'mvn sonar:sonar -Dsonar.host.url=http://35.153.67.119:9000 -Dsonar.login=77467cfd2653653ad3b35463fbfdb09285f08be5'
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage ('Deployment'){
		deploy adapters: [tomcat9(credentialsId: 'Tomcatcreds', path: '',  url: 'http://ec2-54-152-29-157.compute-1.amazonaws.com:8080/')], contextpath: null, war: 'target/*.war'
	}
	
	stage ('Notification'){
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "isasalakr@gmail.com"
		    )
	}
}

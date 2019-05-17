#!/usr/bin/groovy
pipeline {
    environment {
        JAVA_HOME = tool('java')
    }
agent any
options {
disableConcurrentBuilds()
}
stages {
	stage("Mule4-Calculator-DEV") {
		stages {  
			stage("Build : Mule4-Calculator-DEV Source Code") {
				steps {
					slackSend (color: "#f1502f", message: "Git URL is : ${env.GIT_URL}")
					slackSend (color: "add8e6", message: 'Calculator-munit-mule4 Deployment Started')
					dir ('.' ) {
					sh '/devops/maven/apache-maven-3.3.9/bin/mvn clean package mule:deploy'
					}
				}
			}	

			stage('Upload DEV Artifacts To Artifactory') {
				steps {
					script{
					sh "echo ${env.GIT_URL} > /tmp/giturl.txt"
					def server = Artifactory.server 'artifactory'
					def uploadSpec = """{
					"files": [
							{
								"pattern": "**/*.jar",
								"target": "generic-local/Calculator-munit-mule4_DEV_$BUILD_NUMBER/Calculator-munit-mule4.jar"
							}
							]
					}"""                 
					def buildInfo1 = server.upload spec: uploadSpec

					}
				}
			}  	
		}
		post {
			failure {
					slackSend (color: "0000ff", message: 'Calculator-munit-mule4 Build failed')
				emailext attachLog: true, body: '''The Failed build details are as follows:<br> <br>
				<table border="1">
				<tr><td style="background-color:white;color:red"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
				<tr><td style="background-color:white;color:red"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
				<tr><td style="background-color:white;color:red"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
				<tr><td style="background-color:white;color:red"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
				</table>
				''', subject: 'ami-api Deployment Status', to: 'srikanth.bathini@eaiesb.com'
					slackSend (color: "#FF0001",message: 'Calculator-munit-mule4 Deployment Failed')
			}
			success {
					slackSend (color: "0000ff", message: 'Calculator-munit-mule4 Build sucess')
					slackSend (color: "#FFA500",message: 'Calculator-munit-mule4 Uploaded Sucessfully')
				emailext attachLog: true, mimeType: 'text/html', body: '''The jenkins build details are as follows:<br> <br>
				<table border="1">
				<tr><td style="background-color:#33339F;color:white"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
				<tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
				<tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
				<tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
				</table>
				''', subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME} ${ENV, var="GIT_URL"}', to: 'srikanth.bathini@eaiesb.com'    
					slackSend (color: "#32CD32", message: 'Calculator-munit-mule4 Deployment is Sucessful')
			}
		}
	}
	
	stage("Mule4-Calculator-QA") {
		stages {
			stage ('Approve?')
				{
					timeout(time:2, unit:'DAYS')
					{
						input message: 'Waiting For Approval ??', submitter: QAGroup
					}
			}			
			stage("Build : Mule4-Calculator-QA Source Code") {
				steps {
					slackSend (color: "#f1502f", message: "Git URL is : ${env.GIT_URL}")
					slackSend (color: "add8e6", message: 'Calculator-munit-mule4 Deployment Started')
					dir ('.' ) {
					sh '/devops/maven/apache-maven-3.3.9/bin/mvn clean package mule:deploy'
					}
				}
			}	

			stage('Upload QA Artifacts To Artifactory') {
				steps {
					script{
					sh "echo ${env.GIT_URL} > /tmp/giturl.txt"
					def server = Artifactory.server 'artifactory'
					def uploadSpec = """{
					"files": [
							{
								"pattern": "**/*.jar",
								"target": "generic-local/Calculator-munit-mule4_QA_$BUILD_NUMBER/Calculator-munit-mule4.jar"
							}
							]
					}"""                 
					def buildInfo1 = server.upload spec: uploadSpec

					}
				}
			}  	
		}
		post {
			failure {
					slackSend (color: "0000ff", message: 'Calculator-munit-mule4 Build failed')
				emailext attachLog: true, body: '''The Failed build details are as follows:<br> <br>
				<table border="1">
				<tr><td style="background-color:white;color:red"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
				<tr><td style="background-color:white;color:red"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
				<tr><td style="background-color:white;color:red"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
				<tr><td style="background-color:white;color:red"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
				</table>
				''', subject: 'ami-api Deployment Status', to: 'srikanth.bathini@eaiesb.com'
					slackSend (color: "#FF0001",message: 'Calculator-munit-mule4 Deployment Failed')
			}
			success {
					slackSend (color: "0000ff", message: 'Calculator-munit-mule4 Build sucess')
					slackSend (color: "#FFA500",message: 'Calculator-munit-mule4 Uploaded Sucessfully')
				emailext attachLog: true, mimeType: 'text/html', body: '''The jenkins build details are as follows:<br> <br>
				<table border="1">
				<tr><td style="background-color:#33339F;color:white"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
				<tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
				<tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
				<tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
				</table>
				''', subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME} ${ENV, var="GIT_URL"}', to: 'srikanth.bathini@eaiesb.com'    
					slackSend (color: "#32CD32", message: 'Calculator-munit-mule4 Deployment is Sucessful')
			}
		}
	}

	stage("Mule4-Calculator-PROD") {
		stages {  
			stage("Build : Mule4-Calculator-PROD Source Code") {
				stage ('Approve?')
					{
						timeout(time:2, unit:'DAYS')
						{
							input message: 'Waiting For Approval ??', submitter: PRODGroup
						}
				}						
				steps {
					slackSend (color: "#f1502f", message: "Git URL is : ${env.GIT_URL}")
					slackSend (color: "add8e6", message: 'Calculator-munit-mule4 Deployment Started')
					dir ('.' ) {
					sh '/devops/maven/apache-maven-3.3.9/bin/mvn clean package mule:deploy'
					}
				}
			}	

			stage('Upload PROD Artifacts To Artifactory') {
				steps {
					script{
					sh "echo ${env.GIT_URL} > /tmp/giturl.txt"
					def server = Artifactory.server 'artifactory'
					def uploadSpec = """{
					"files": [
							{
								"pattern": "**/*.jar",
								"target": "generic-local/Calculator-munit-mule4_PROD_$BUILD_NUMBER/Calculator-munit-mule4.jar"
							}
							]
					}"""                 
					def buildInfo1 = server.upload spec: uploadSpec

					}
				}
			}  	
		}
		post {
			failure {
					slackSend (color: "0000ff", message: 'Calculator-munit-mule4 Build failed')
				emailext attachLog: true, body: '''The Failed build details are as follows:<br> <br>
				<table border="1">
				<tr><td style="background-color:white;color:red"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
				<tr><td style="background-color:white;color:red"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
				<tr><td style="background-color:white;color:red"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
				<tr><td style="background-color:white;color:red"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
				</table>
				''', subject: 'ami-api Deployment Status', to: 'srikanth.bathini@eaiesb.com'
					slackSend (color: "#FF0001",message: 'Calculator-munit-mule4 Deployment Failed')
			}
			success {
					slackSend (color: "0000ff", message: 'Calculator-munit-mule4 Build sucess')
					slackSend (color: "#FFA500",message: 'Calculator-munit-mule4 Uploaded Sucessfully')
				emailext attachLog: true, mimeType: 'text/html', body: '''The jenkins build details are as follows:<br> <br>
				<table border="1">
				<tr><td style="background-color:#33339F;color:white"><b>Job Name</b></td><td>$JOB_NAME</td></tr>
				<tr><td style="background-color:#33339F;color:white"><b>Build Number</b></td><td>$BUILD_NUMBER</td></tr>
				<tr><td style="background-color:#33339F;color:white"><b>GIT URL</b></td><td>${FILE, path="/tmp/giturl.txt"}</td></tr>
				<tr><td style="background-color:#33339F;color:white"><b>Build URL</b></td><td>$BUILD_URL</td></tr>
				</table>
				''', subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME} ${ENV, var="GIT_URL"}', to: 'srikanth.bathini@eaiesb.com'    
					slackSend (color: "#32CD32", message: 'Calculator-munit-mule4 Deployment is Sucessful')
			}
		}
	}	
}
}

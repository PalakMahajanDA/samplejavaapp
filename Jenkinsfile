def workspace = "home\\jagenthome"
def server = "TProject2"

pipeline {
   agent {
    node {
      label server
	  customWorkspace workspace
    }
  }
    stages {
        stage('compile') {
	   steps {
                echo 'compiling..'
		git url: 'https://github.com/lerndevops/samplejavaapp'
		sh script: '/opt/apache-maven-3.8.5/bin/mvn compile'
           }
        }
        stage('unit-test') {
	   steps {
                echo 'unittest..'
	        sh script: '/opt/apache-maven-3.8.5/bin/mvn test'
                 }
	   post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
           }			
        }
        stage('package') {
	   steps {
                echo 'package......'
		sh script: '/opt/apache-maven-3.8.5/bin/mvn package'	
           }		
        }
    }
}

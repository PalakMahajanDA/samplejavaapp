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
	     stage('build & push docker image') {
	         steps {
              withDockerRegistry(credentialsId: 'DOCKER_HUB_LOGIN', url: 'https://index.docker.io/v1/') {
                    sh script: 'cd  $WORKSPACE'
                    sh script: 'docker build --file Dockerfile --tag docker.io/lerndevops/samplejavaapp:$BUILD_NUMBER .'
                    sh script: 'docker push docker.io/lerndevops/samplejavaapp:$BUILD_NUMBER'
              }	
           }		
        }
	      stage('deploy-Server') {
	         steps {
                    sh script: 'sudo ansible-playbook --inventory /tmp/myinv $WORKSPACE/deploy/deploy-kube.yml --extra-vars "env=qa build=$BUILD_NUMBER"'
           }		
        }
    }
}

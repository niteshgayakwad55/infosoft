pipeline {
    agent any

//	tools {
//		jdk 'jdk8'
//	}
//	environment {
//		M2_INSTALL = "/usr/bin/mvn"
//	}

    stages {
        stage('Clone-Repo') {
	    steps {
	        checkout scm
	    }
        }

        stage('Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=true'
            }
        }
		
        stage('Unit Tests') {
            steps {
                sh 'mvn compiler:testCompile'
                sh 'mvn surefire:test'
                junit 'target/**/*.xml'
            }
        }

        stage('Deployment') {
            steps {
                sh 'sshpass -p "raghu" scp target/amazon.war raghu@172.17.0.2:/home/raghu/Distros/apache-tomcat-9.0.78/webapps'
                sh 'sshpass -p "raghu" ssh raghu@172.17.0.2 "/home/raghu/Distros/apache-tomcat-9.0.78/bin/startup.sh"'
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
				checkout([$class: 'GitSCM', branches: [[name: 'main']], extensions: [[$class: 'CheckoutOption', timeout: 5], [$class: 'CloneOption', noTags: false, reference: '', shallow: false, timeout: 5]], userRemoteConfigs: [[url: 'https://github.com/vinodm0502/devops.git']]])
                echo 'Checkout source from git'
            }
        }
        stage('Quality Check') {
            steps {
                echo 'jobparm $(JobName)'
                echo 'Quality Check'
            }
        }
        stage('Security Check') {
            steps {
		dependencyCheck additionalArguments: '--scan=. --format=HTML', odcInstallation: 'OWAPS-Dependency-Check'
                echo 'Security Check'
            }
        }
	stage('Build Push App') {
		steps {
			sh "mvn clean install"
			echo 'App push done'
		}
	}
        stage('Deploy App') {
            steps {
		sh "JENKINS_NODE_COOKIE=dontKillMe nohup java -jar target/spring-boot-rest-2-0.0.1-SNAPSHOT.jar &"
                echo 'Deploy App'
            }
        }
        stage('Post Deployment Check') {
            steps {
                echo 'Post Deployment Check'
            }
        }
    }
}

pipeline {
    environment {
	registry = "docker.io/maho/myweb"
	dockerImage = ""
    }
    
    agent any

    stages {
	
	stage('Checkout Source') {
	    steps {
		git 'https://github.com/takara9/jenkins-play-1'
	    }
	}
	stage('Build image') {
	    steps {
		script {
		    dockerImage = docker.build registry + ":$BUILD_NUMBER"
		}
	    }
	}
	stage('Push image') {
	    steps {
		script {
		    docker.withRegistry("https://index.docker.io/v1/","dockerhub") {
			dockerImage.push()
		    }
		}
	    }
	}
	stage('Deploy App') {
	    steps {
		script {
		   sh `pwd`
		   sh `date`
		   #kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
		}
	    }
	}
    }
}

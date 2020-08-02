pipeline {
    environment {
	registry = "docker.io/maho/myweb"
	dockerImage = ""
	KUBECONFIG = credentials('kubeconfig')
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
                   //sh 'echo $KUBECONFIG'
		   sh 'kubectl cluster-info --kubeconfig $KUBECONFIG'
		   sh 'sed s/__BUILDNUMBER__/$BUILD_NUMBER/ myweb.yaml > deploy_myweb.yaml'
		   sh 'kubectl apply -f deploy_myweb.yaml --kubeconfig $KUBECONFIG'
		   //kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
		}
	    }
	}
    }
}

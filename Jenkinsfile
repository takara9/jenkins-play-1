pipeline {
    //
    //  Jenkinsサーバーにdocker, kubectl コマンドが導入されていること
    //  プラグイン kubernetes-cd は使用しない。
    //  プラグイン Blue Ocean、docker-build-step が必須
    environment {
	registry = "harbor.labs.local/library/myweb"
	dockerImage = ""
	KUBECONFIG = credentials('kubeconfig')
    }
    
    agent any

    stages {
	stage('GiHubからソースコードのクローン') {
	    steps {
		git 'https://github.com/takara9/jenkins-play-1'
	    }
	}
	stage('コンテナイメージのビルド') {
	    steps {
		script {
		    dockerImage = docker.build registry + ":$BUILD_NUMBER"
		}
	    }
	}
	stage('コンテナレジストリへプッシュ') {
	    steps {
		script {
		    docker.withRegistry("https://harbor.labs.local/v1/","harborproj1") {
			dockerImage.push()
		    }
		}
	    }
	}
	stage('K8sクラスタへのデプロイ') {
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

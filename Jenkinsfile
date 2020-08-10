pipeline {
  agent {
    docker {
        image 'docker'
	args '-v /var/run/docker.sock:/var/run/docker.sock -u root' 
    }


  }
  stages {
    stage('Validate Environment') {
      steps {
        sh '''
	ls
	pwd
	env
	whoami
	'''
      }
    }

    stage('manifest artifact') {
      steps {
        timeout(time: 10, unit: 'MINUTES') {
	  sh "echo 'time :' 0 > manfist.bible"
	  sh "echo 'build number :' ${BUILD_NUMBER} >> manfist.bible"
	  sh "echo 'built by : fish pipeline ' >> manfist.bible"
        }

      }
    }

    stage('build') {
      steps {
        sh ''' 
	docker build -t lordblackfish/evil-image:$BUILD_NUMBER .
        docker image ls
        docker run --rm lordblackfish/evil-image:${BUILD_NUMBER} cat manfist.bible
	'''
      }
    }

    stage('push to dockerhub') {
      steps {
        sh '''
        echo "$DOCKER_PSW" | docker login --username $DOCKER_ID --password-stdin
	docker push lordblackfish/evil-image:${BUILD_NUMBER}
	docker image rm lordblackfish/evil-image:${BUILD_NUMBER}
       '''
      }
    }


    stage('test artifact') {
      steps {
        sh '''
        docker run --rm lordblackfish/evil-image:$latest cat manfist.bible
       '''
      }
    }


  }
  environment {
    CREDS = credentials('docker-creds')
    DOCKER_ID = "$CREDS_USR"
    DOCKER_PSW = "$CREDS_PSW"
    OWNER = 'jenkins'
  }
}

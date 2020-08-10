pipeline {
  agent {
    docker {
        image 'docker'
	args '-v /var/run/docker.sock:/var/run/docker.sock' 
    }


  }
  stages {
    stage('Validate Environment') {
      steps {
        sh '''
ls
pwd
env
'''
      }
    }

    stage('manifest artifact') {
      steps {
        timeout(time: 10, unit: 'MINUTES') {
	  sh "echo 'time :' ${BUILD_ID} > manfist.bible"
	  sh "echo 'build number :' ${BUILD_NUMBER} >> manfist.bible"
	  sh "echo 'built by : fish pipeline '"
        }

      }
    }

    stage('build') {
      steps {
        sh "docker build -t evil-image:$BUILD_NUMBER ."
      }
    }

    stage('push to reg') {
      steps {
        sh '''
	docker image ls
	docker run --rm evil-image:${BUILD_NUMBER} cat manfist.bible
        docker login -u lordblackfish -p r28RW3E5i4Ex
	docker push lordblackfish/evil-image:${BUILD_NUMBER}
	docker image rm evil-image:${BUILD_NUMBER}
        docker run --rm lordblackfish/evil-image:${BUILD_NUMBER} cat manfist.bible
'''
      }
    }

  }
  environment {
    OWNER = 'jenkins'
  }
}

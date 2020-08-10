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
          sh "echo 'time' > manfist.bible"
	  sh "echo ${BUILD_ID} >> manfist.bible"
	  sh "echo 'build number :' ${BUILD_NUMBER} >> manfist.bible"
        }

      }
    }

    stage('build') {
      steps {
        sh "docker build -t $BUILD_NUMBER ."
      }
    }

    stage('Release') {
      steps {
        sh '''
date 
ls 
pwd
docker image ls
'''
      }
    }

  }
  environment {
    OWNER = 'jenkins'
  }
}

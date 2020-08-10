pipeline {
  agent {
    docker {
        image 'bryandollery/terraform-packer-aws-alpine'
        args  '-v /var/run/docker.sock:/var/run/docker.sock -it'
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

    stage('build') {
      steps {
        timeout(time: 10, unit: 'MINUTES') {
          sh 'ls -a'
        }

      }
    }

    stage('Test') {
      steps {
        git(url: 'https://github.com/KnowledgeHut-AWS/jenkins', branch: 'master')
      }
    }

    stage('Release') {
      steps {
        sh '''
date 
ls 
pwd

'''
      }
    }

  }
  environment {
    OWNER = 'jenkins'
  }
}

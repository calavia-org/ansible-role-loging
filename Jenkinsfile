pipeline {
  agent {
    docker {
      image 'robertdebock/github-action-molecule'
      args '-u 0 -v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  stages {

    stage ('Display versions') {
      steps {
        sh '''
          ansible --version
          molecule --version
        '''
      }
    }

    stage ('Molecule test') {
      steps {
        sh 'sudo molecule test --all'
      }
    }

  } // close stages
  post {
      always {
        junit '**/reports/junit/*.xml'
      }
   } 
}   // close pipeline


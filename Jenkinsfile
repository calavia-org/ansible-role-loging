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
        sh 'molecule lint'
      }
      steps {
        sh 'molecule destroy'
      }
      steps {
        sh 'molecule create'
      }
      steps {
        sh 'molecule prepare'
      }
      steps {
        sh 'molecule converge'
      }
      steps {
        sh 'molecule idempotence'
      }
      steps {
        sh 'ANSIBLE_STDOUT_CALLBACK=junit JUNIT_OUTPUT_DIR="junit-results" JUNIT_FAIL_ON_CHANGE=true JUNIT_HIDE_TASK_ARGUMENTS=true molecule verify'
      }
      steps {
        sh 'molecule destroy'
      }
    }

  } // close stages
  post {
      always {
        junit '**/reports/junit/*.xml'
        sh 'molecule destroy'
      }
   } 
}   // close pipeline


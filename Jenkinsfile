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
        sh '''
          molecule lint
          molecule destroy
          molecule create
          molecule prepare
          molecule converge
          molecule idempotence
          ANSIBLE_STDOUT_CALLBACK=junit JUNIT_OUTPUT_DIR="molecule/default/reports/junit" JUNIT_FAIL_ON_CHANGE=true JUNIT_HIDE_TASK_ARGUMENTS=true molecule verify
        '''
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


pipeline {
  agent any 
  tools {
            maven 'Maven'
        }
  stages {
    stage ('Initialize') {
                            steps {
                                    sh '''
                                           echo "PATH = ${PATH}"
                                          echo "M2_HOME = ${M2_HOME}"
                                      ''' 
                                  }
                          }
    stage ('Check-Git-Secrets') {
      steps {
        sh 'sudo rm trufflehog || true'
        sh 'sudo docker run gesellix/trufflehog --json https://github.com/cehkunal/webapp.git > trufflehog'
        sh 'sudo cat trufflehog'
      }
    }
        
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
  }
}

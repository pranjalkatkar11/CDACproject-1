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
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
     stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'unset ssh_auth_sock ssh_agent_pid'
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@34.201.70.48:/prod/apache-tomcat-9.0.79/webapps/webapp.war'
              }   
            }
     }
  }
}

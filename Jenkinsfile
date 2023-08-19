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
                sh 'scp -v -o StrictHostKeyChecking=no -r target/*.war ubuntu@172.31.89.52:/prod/apache-tomcat-9.0.79/webapps/webapp.war'
              }   
            }
     }
  }
}

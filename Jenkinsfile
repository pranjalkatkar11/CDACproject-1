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
           sshagent(['Tomcat']) {
               sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@44.212.1.10:/prod/apache-tomcat-9.0.79/webapps/webapp.war'
              }      
           }       
    }
  }
}

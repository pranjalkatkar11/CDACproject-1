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
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/ArnabDhar/CADCProject.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
      stage ('Source Composition Analysis') {
       steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/ArnabDhar/CADCProject/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
         
        }
      }
       stage('SAST') {
            steps {
                script {
                    def auth_token = 'YOUR_GENERATED_TOKEN' // Replace with your actual authentication token
                    
                    withSonarQubeEnv('sonar') {
                        sh "mvn sonar:sonar -Dsonar.login=${auth_token}"
                        sh 'cat target/sonar/report-task.txt'
                    }
                }
            }
        }
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
   stage ('Deploy-To-Tomcat') {
            steps {
            sh 'scp -A target/*.war  jenkins@172.31.83.179:/prod/apache-tomcat-9.0.79/webapps/webapp.war'  
          }       
    }
  }
}

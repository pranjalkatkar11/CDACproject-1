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
                    def auth_token = 'sqa_ef5e99ace0a414a798d07bb9061766f872f06e61' // Replace with your actual authentication token
                    
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

    stage ('DAST') {
      steps {
         sh 'ssh  -A owaspzap@172.31.82.131 "ls"'
        sh 'ssh owaspzap@172.31.82.131 -t "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://3.83.206.66:8080/webapp/"'
      }
    }
  }
}

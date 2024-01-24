pipeline {
    agent any
    
environment {
        SONARQUBE_SCANNER_HOME = tool 'sonarqube-10.3'
        SONARQUBE_URL = 'http://10.10.30.117:9000' // Replace with your SonarQube server URL
    }
    
    stages {
        stage('SCM') {
            steps {
                git 'https://github.com/mansib24/dvja.git'
            }
        }
        stage('DP Check') {
           steps {
        dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
--prettyPrint''', odcInstallation: 'owasp-dependency-checker'
        
        dependencyCheckPublisher pattern: 'dependency-check-report.xml'
      }
        }

        stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/mansib24/dvja.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
       stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis
                    //docker.image('sonarqube:latest').withRun('-p 9003:9003') { 
                        // Assuming your SonarQube server is running on port 9003
                        sh "mvn clean sonar:sonar \
                            sonar.host.url=http://10.10.30.117:9000 \
                            sonar.login=sqp_662bf263dd46cbdfecfccc38b06a67049748edf9"
                    
                }
            }
        }

  
    }
}

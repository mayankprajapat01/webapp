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

    stage('Check-Git-Secrets'){
      steps{
        sh 'rm trufflehog || true'
        sh 'docker run trufflesecurity/trufflehog git https://github.com/mayankprajapat01/webapp.git --json > trufflehog'
        sh 'cat trufflehog'
      }
    }

    stage('Source-Composition-Analysis'){
      steps{
        dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DP-Check'
        dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
      }
    }
   
    stage ('Build') {
      steps {
      sh '''
                    mvn clean package
         '''
       }
    }

    //change ip everytime you restart your ec2 instance
    stage('Deploy-to-Tomcat'){
      steps{
        sshagent(['tomcat']){
           sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.50.248.234:/home/ubuntu/prod/apache-tomcat-9.0.100/webapps'
        }
      }
    }
  }    
}

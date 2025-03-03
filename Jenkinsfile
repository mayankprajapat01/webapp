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

    // stage('Check-Git-Secrets'){
    //   steps{
    //     sh '''
    //       docker run trufflesecurity/trufflehog git https://github.com/mayankprajapat01/webapp.git --json
    //     '''
    //   }
    // }
   
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
           sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@16.170.168.242:/home/ubuntu/prod/apache-tomcat-9.0.100/webapps'
        }
      }
    }
  }    
}

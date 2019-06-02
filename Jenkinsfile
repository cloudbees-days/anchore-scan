pipeline {
  agent { label 'default-jnlp' }
  
  triggers {
    eventTrigger jmespathQuery("eventName=='containerImagePush'")
  }
  
  stages {
    stage('Anchore Scan') {
      steps {
        script {
          echo currentBuild.getBuildCauses()[0].event.source.toString()
        }
      }
    }
  }
}

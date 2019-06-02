pipeline {
  agent { label 'default-jnlp' }
  
  triggers {
    eventTrigger jmespathQuery("eventType=='containerImagePush'")
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

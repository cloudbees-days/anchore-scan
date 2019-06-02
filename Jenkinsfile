pipeline {
  agent { label 'default-jnlp' }
  
  triggers {
    eventTrigger jmespathQuery("eventType=='containerImagePush'")
  }
  
  stages {
    stage('Anchore Scan') {
      steps {
        script {
          if (currentBuild.getBuildCauses("com.cloudbees.jenkins.plugins.pipeline.events.EventTriggerCause").size() > 0) {
            eventTriggerCause = currentBuild.getBuildCauses()[0].event.toString()
            containerImage = sh(script: """
               eventTriggerCause | jq '.image'
            """, returnStdout: true)
            echo containerImage
          }
        }
      }
    }
  }
}
